# Writing a GitHook in Node
## Cheat sheet
Your Node GitHook can access the following properties:

```javascript
var request, // useful RESTful module for HTTP requests
    succeed, // githooks must call this to end the request
    fail,    // or this!
    ACCESS_TOKEN, // the access token of the user who installed your GitHook
    GET,          // the parameters you set up, e.g. GET.template.
    PAYLOAD       // the payload JSON object originally sent by GitHub
```

## Constants
You'll want to make use of these constants in your code:

```
ACCESS_TOKEN // the access token of the user who installed your GitHook
GET          // the parameters you set up, e.g. GET.template.
PAYLOAD      // the payload JSON object originally sent by GitHub
```

## GitHook termination
Your GitHook must explicitly say when it has finished. It can reveal it succeeded or failed depending on which method you call. At the moment this doesn't really achieve much, but helps GitHooks.io monitor the proportion of successful and unsuccessful calls.

To say that your GitHook has finished, you need to call one of the following methods:

```
succeed('All was good!');
fail('This failed for some reason!');
```

## Available modules
Our infrastructure has [Node's 'request' HTTP Client](https://github.com/request/request) installed, which you can make use of in your code (accessible via `request`).

A quick example is shown below:

```
var options = {
    url:      PAYLOAD.pull_request.url,
    headers: {
        'Content-Type':  'application/json',
        'User-Agent':    'some-user-agent',
        'Authorization': 'token ' + ACCESS_TOKEN
    },
    json: {
        "body": PAYLOAD.pull_request.body + "\n" + body
    }
};

request.post(options, function templatePosted(err, httpResponse, body) {
    if (err) {
        fail('Could not send POST request: ' + err);
    }
    else {
        succeed('Template POST message successful. Response:' + body);
    }
});
```

## Example
An example implementation of a GitHook is available at [https://github.com/GitHooksIO/githook-pr-editor](https://github.com/GitHooksIO/githook-pr-editor).

## Miscellaneous
GitHooks.io runs its GitHooks on AWS Lambda infrastructure. You can find out more about the [Node Lambda Programming model](http://docs.aws.amazon.com/lambda/latest/dg/programming-model.html).
