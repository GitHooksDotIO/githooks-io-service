# Writing a GitHook in Node
## Get started
GitHooks.io runs its GitHooks on AWS Lambda infrastructure. You can [find out more about the Node Lambda Programming model](http://docs.aws.amazon.com/lambda/latest/dg/programming-model.html).

Whilst Lambda is awesome, it has its limitations. Crucially, **your GitHook must be entirely self-contained** (i.e. have no external dependencies), except for the modules made available to you in the `gh.modules` parameter. You CANNOT require any custom modules in the same repository. Your GitHook must be entirely self-contained in the file you specified as your entry point.

That's really all you need to know about the infrastructure. Ready to start? There is a [boilerplate vanilla project](https://github.com/GitHooksIO/boilerplate-githook) just waiting to be cloned and worked on!

## Schema
Your GitHook module will be passed a parameter which is split into three sets of utilities: `data`, `modules`, and `process`. An overview is presented below, but there is more detail in the sections that follow.

```javascript
module.exports = function (gh) {

    /* gh is equal to:
    {
        data: {
            payload:    (JSON), // the original GitHub payload information
            parameters: (JSON), // any custom parameters the user has installed your GitHook with
        },
        modules: {
            request:     (Node module), // Useful HTTP request client. See https://www.npmjs.com/package/request
            authRequest: (Node module), // GitHook's own HTTP request client, which automatically attaches access token with each request
        },
        process: {
            succeed: (function), // tell GitHooks.io that your GitHook has completed successfully
            fail:    (function), // tell GitHooks.io that your your GitHook ran into an issue
        },
    }
    */
    
    gh.process.succeed('Nothing to see here!');
};
```

## Data

`data.payload` contains the [payload object](https://developer.github.com/webhooks/#payloads) sent by GitHub.com. You'll use this to find out what kind of event has just triggered your GitHook, and how you should respond.

`data.parameters` is a JSON object containing any user-supplied custom parameters for your GitHook. [Read more about GitHook parameters](http://githooks.io/parameters).

## Modules

`modules.request` is a [request](https://github.com/request/request) (HTTP client) Node module, which you can make use of as per the linked instructions. Use `request` for any HTTP requests you need to make.

...EXCEPT for [GitHub API](https://developer.github.com/v3/) requests, which should be fed through the `modules.authRequest` module. This is a custom module maintained by GitHooks.io, and automatically passes the correct access token along with the API request. `authRequest` has a few public methods:

```js
var authRequest = gh.modules.authRequest;

/* available methods:
authRequest.get;
authRequest.post;
authRequest.put;
authRequest.patch;
authRequest.del;
*/

// example
authRequest.post({
    url: gh.data.payload.pull_request.url,
    json: {
        "body": "I am overwriting the contents of the PR!"
    }
}, function templatePosted(err, httpResponse, body) {
    if (err) {
        gh.process.fail('Could not send POST request: ' + err);
    }
    else {
        gh.process.succeed('POST message was successful. Response:' + JSON.stringify(body));
    }
});
```

In the `authRequest.post` example above, the `authRequest` module will automatically append the following headers to the request:

```
'Content-Type':  'application/json',
'User-Agent':    'githooks-io',
'Authorization': 'token abc123r3n9nf9ndsfn8sw9gnvs8d9bn' // the user's access token
```

## Process
Your GitHook must explicitly say when it has finished. It can reveal it succeeded or failed depending on the method that you call. At the moment this doesn't really achieve much, but helps GitHooks.io monitor the proportion of successful and unsuccessful calls.

To say that your GitHook has finished, you need to call one of the following methods:

```js
gh.process.succeed('All was good!');
gh.process.fail('This failed for some reason!');
```

There may be cases where your GitHook doesn't really need to do anything. For instance, you may be listening for a Pull Request being opened, so your GitHook gets triggered by the `pull_request` event - but this event gets sent whether a Pull Request was `opened`, `closed`, `assigned`, etc. In these cases, we recommend that you call `process.succeed`, because the process DID succeed; it just didn't have to act upon the information it was given.

`process.fail` should be reserved for situations where the process was unable to complete - e.g. a third party service your GitHook relies upon gave a temporary 500 error.

Error checking is good, because you can explicitly call `fail` if you get an error - but don't feel that you have to be overly zealous with your error checking. GitHooks.io will check a lot of things for you - such as whether or not the user's access token grants sufficient privileges as was specified in your `.githook.yml` file.
