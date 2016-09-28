# Writing a GitHook in Node
## Get started
There is a [boilerplate vanilla project](https://github.com/GitHooksIO/boilerplate-githook) just waiting to be cloned and worked on!

## Cheat sheet
Here is an example Node GitHook, with all the defined properties it can access:

```javascript
module.exports = function (data, process) {

    // Useful HTTP request client. See https://www.npmjs.com/package/request
    var request = require('request');
    
    // the original GitHub payload information (JSON), forwarded to your GitHook
    data.payload;

    // any custom parameters the user has installed your GitHook with. Access like data.parameters.my_template
    data.parameters;   

    // the access token of the user who installed your GitHook (you'll want to pass this to your GitHub API requests)
    data.access_token; 

    // tell GitHooks.io that your GitHook has completed successfully
    process.succeed(); 

    // tell GitHooks.io that your GitHook ran into an issue
    process.fail();    

};
```

## GitHooks Infrastructure
GitHooks.io runs its GitHooks on AWS Lambda infrastructure. You can [find out more about the Node Lambda Programming model](http://docs.aws.amazon.com/lambda/latest/dg/programming-model.html).

Whilst Lambda is awesome, it has its limitations. Crucially, **your GitHook must be entirely self-contained** (i.e. have no external dependencies), except for the extremely limited list of available Node modules (below) which we have pre-installed on the Lambda function.

You CANNOT require any custom modules in the same repository. Your GitHook must be _entirely self-contained_ in the file you specified as your entry point.

### Available Node modules
We currently have the following Node modules available to your GitHook:

* [request](https://github.com/request/request) (HTTP client), which you can make use of as per the linked instructions. Pull into your GitHook as follows: `var request = require('request')`. We strongly recommend using this module to call the [GitHub API](https://developer.github.com/v3/).

## GitHook termination
Your GitHook must explicitly say when it has finished. It can reveal it succeeded or failed depending on which method you call. At the moment this doesn't really achieve much, but helps GitHooks.io monitor the proportion of successful and unsuccessful calls.

To say that your GitHook has finished, you need to call one of the following methods:

```js
process.succeed('All was good!');
process.fail('This failed for some reason!');
```

There may be cases where your GitHook doesn't really need to do anything. For instance, you may be listening for a Pull Request being opened, so your GitHook gets triggered by the `pull_request` event - but this event gets sent whether a Pull Request was opened, closed, assigned, etc. In these cases, we recommend that you call `process.succeed`, because the process did indeed succeed; it just didn't have to act upon the information it was given.
