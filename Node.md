# Writing a GitHook in Node
## Get started
There is a [boilerplate/skeleton-code/hello-world project](https://github.com/GitHooksIO/boilerplate-githook) just waiting to be cloned and worked on!

## Cheat sheet
Your Node GitHook can access the following properties:

```javascript
module.exports = function (data, process) {
    request;           // Useful HTTP request client. See https://www.npmjs.com/package/request
    data.payload;      // the original GitHub payload information (JSON), forwarded to your GitHook
    data.parameters;   // any custom parameters the user has installed your GitHook with. Access like data.parameters.my_template
    data.access_token; // the access token of the user who installed your GitHook
    process.succeed(); // tell GitHooks.io that your GitHook has completed successfully
    process.fail();    // tell GitHooks.io that your GitHook ran into an issue
};
```

## GitHook termination
Your GitHook must explicitly say when it has finished. It can reveal it succeeded or failed depending on which method you call. At the moment this doesn't really achieve much, but helps GitHooks.io monitor the proportion of successful and unsuccessful calls.

To say that your GitHook has finished, you need to call one of the following methods:

```
process.succeed('All was good!');
process.fail('This failed for some reason!');
```

## Available modules
Our infrastructure has [Node's 'request' HTTP Client](https://github.com/request/request) installed, which you can make use of in your code (accessible via `request`).

## Example
An example implementation of a GitHook is available at [https://github.com/GitHooksIO/githook-pr-editor](https://github.com/GitHooksIO/githook-pr-editor).

## Miscellaneous
GitHooks.io runs its GitHooks on AWS Lambda infrastructure. You can find out more about the [Node Lambda Programming model](http://docs.aws.amazon.com/lambda/latest/dg/programming-model.html).
