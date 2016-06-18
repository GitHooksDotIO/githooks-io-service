# How It Works
We'll explain how GitHooks.io works by going through a worked example: the [PR Editor GitHook](http://githooks.io/githooks/GitHooksIO/githook-pr-editor). We'll cover every step a user would take in getting the GitHook installed and seeing it working.

The PR Editor allows you to provide a template which will be automatically appended to every new Pull Request. This is intended for appending checklists to Pull Requests. In February 2016, GitHub provided this functionality natively, so we no longer recommend using this GitHook - but it is still a good example for the purposes of this guide.

## Background information
Every GitHook contributed to GitHooks.io has its own row in the database, detailing the owner (in this case, `GitHooksIO`) and the GitHook name (in this case, `githook-pr-editor`), corresponding to the repository and repository owner on GitHub.com.

We call the GitHook repository the **provider**.

## Signing into GitHooks.io
You're a user with a repository, and want to install a GitHook. *Your* repository is the **consumer**. The first step is creating an account on GitHooks.io - which is as easy as clicking 'Sign in with GitHub'.

## Installing a webhook
You go to the GitHook page, and can see an 'Install' section where you can choose which repository you want to install the GitHook to.

Some GitHooks also have parameters you can play with. For instance, the PR Editor GitHook requires a URL to a markdown/text file which is to be appended to new Pull Requests.

![screenshot of user input form](https://cloud.githubusercontent.com/assets/16819594/16172564/8bcfb37c-3584-11e6-9a34-286fa061b043.png)

You hit 'Install GitHook': this creates a new webhook inside your repository settings.

![screenshot of webhook entry on github](https://cloud.githubusercontent.com/assets/16819594/16172662/a987ea56-3584-11e6-8c3f-d9668e90aaaa.png)

The webhook's payload URL is pre-defined for you, as is the content type (which currently has to be `application/x-www-form-urlencoded`), and the Secret. The Secret is completely unique to this GitHook installation on your repository, and will be used later on in the identification process.

![screenshot of webhook config on github](https://cloud.githubusercontent.com/assets/16819594/16172665/b7fed900-3584-11e6-921b-3e316f476f86.png)

The event required by the GitHook is also selected automatically. You (the user) don't have to do anything!

![screenshot of pre-selected webhook event](https://cloud.githubusercontent.com/assets/16819594/16172681/37634e24-3585-11e6-8b5f-cc3c01d8ad11.png)

The GitHook is now active and ready.

## Triggering the GitHook
The webhook will trigger when the necessary event happens (in this case, when a Pull Request is opened).

GitHub makes a POST request to the GitHooks.io endpoint we saw in the above step, specifying headers like this:

```
Request URL: http://githooks.io/githooks/GitHooksIO/githook-pr-editor?template=http%3A%2F%2Fexample.com%2Fpr-checklist.md
Request method: POST
content-type: application/x-www-form-urlencoded
Expect: 
User-Agent: GitHub-Hookshot/0b0c52f
X-GitHub-Delivery: e3c58100-357b-11e6-90f1-2fa5ad909373
X-GitHub-Event: pull_request
X-Hub-Signature: sha1=be4215f96e2a052a290cb510836654a247d05f29
```

The body of the request (known as the 'payload') contains details about the event that has just occurred, e.g.

```json
{
  "action": "opened",
  "number": 4,
  "pull_request": {
    "url": "https://api.github.com/repos/ChrisBAshton/test-webhooks-again/pulls/4",
    "id": 74335976,
    "html_url": "https://github.com/ChrisBAshton/test-webhooks-again/pull/4",
    // etc
```

### Authenticating the webhook
The first thing GitHooks.io will do is get the details of the GitHook which is being executed (the provider). Remember the 'Background information' section at the top of this page? Every GitHook has a record in the GitHooks.io database. So now, we're looking at the URL, we can see that the GitHook requested is the `githook-pr-editor` GitHook owned by `GitHooksIO`, and we want to get the row from the database which corresponds to this.

Assuming we find the row, the row has the GitHook's unique ID. We also have the name of the owner and repository of the consumer - we got this from the payload object. So now we're going through the database finding all installations of this GitHook (provider) to this repository (consumer), as there may be several.

We want to execute the GitHook in the correct context, so it's important we find the *right* record, and not just pick one at random! There may be several installs on the same repository, but with different parameters, or installed by different users who have shared access to the repository (we call the user who installed a given GitHook the GitHook **sponsor**).

So now we're looping through all installation instances on this repository, taking the candidate secret from each row and hashing the payload object and checking if it matches the `X-Hub-Signature` header. Eventually, we find a match.

So now we know:

* Which GitHook to run
* And whose access token to use (that of the *sponsor* which we've just determined)

BUT - there's every chance that the GitHook sponsor has since revoked their permissions, OR the GitHook config has updated and now requires additional permissions. So we first check that the access token has the necessary scope, according to the minimum required scope specified in the GitHooks configuration `.githook.yml` file.

All being well, the access token is suitable, and we can now execute the GitHook.

### Forwarding the request to AWS Lambda
GitHooks.io will now parse the `.githook.yml` configuration to determine which language the webhook is written in (we currently only support Node JS), and where the entry point to the webhook is (i.e. the file we need to execute).

We'll also check to see if all of the requirements have been fulfilled. For instance, the PR Editor GitHook *requires* that a `template` parameter is supplied - it will not run without it. In this case, we check for any `GET` parameters in the payload URL, and can see there is a `template` parameter, so that requirement is fulfilled.

± note to self: this is a bit of a lie - no parameter checking happens until we reach Lambda

With the webhook language determined, we now know which AWS Lambda service to excute our webhook on.

GitHooks.io makes a request to the correct AWS Lambda endpoint, passing:

* the Lambda API key
* the GitHub access token of the GitHook sponsor
* the GitHub repository URL of the GitHook, and the entry point
* all parameters that were in the payload URL
* the payload itself

### Executing on AWS Lambda
Lambda now has all of the information it needs to operate on the webhook request. This is where the magic happens; the code defined by whoever created the GitHook is executed.

GitHooks are usually made up entirely of GitHub API HTTP requests, using the sponsor's access token so that sensitive things (like writing/administrating) can be accomplished. In the case of the PR Editor, we:

* check the payload information - is this the right *type* of Pull Request event? The events get fired when a PR is opened, but also when they're edited or closed. We only want to act if the PR is an `opened` action.
* Assuming this is the right type, we now grab the passed `template` parameter and make a HTTP request to get the file contents...
* Finally, we make a GitHub API request to overwrite the Pull Request body, passing the template contents (appended to the `PAYLOAD.pull_request.body` property so that we don't lose what was already in the PR text.

That's it - the GitHook has executed! This entire operation lasts just a few short seconds.
