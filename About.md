# About GitHooks.io
_GitHooks.io makes it easy to develop, host and consume GitHub webhooks._

GitHooks.io provides:

* a framework in which to write clean, maintainable webhooks
* a hosted environment to serve your webhook endpoints
* a plaform to share your webhooks with other dvelopers

## GitHooks.io as a framework
We provide a framework in which to write your webhooks, taking the complexity out of retrieving access tokens, and with built-in support for parameterising your webhooks.  [Read our Getting Started guide](http://githooks.io/docs-node).

## GitHooks.io as a host
No need to host your webhook on your own servers: GitHooks.io provides a hosted endpoint for your webhook. No setup required - once you've [contributed your GitHook](http://githooks.io/contribute), the endpoint is immediately available for use.

## GitHooks.io as a platform
Once you've contributed your GitHook, anyone can benefit from it - they can install it to their own repositories at the click of a button. GitHooks.io will create the webhook entry on GitHub, securely handle the access token, and pass any custom parameters to the webhook. [Browse our GitHooks](http://githooks.io/githooks) to see what webhooks you might benefit from today!

## Webhooks versus GitHooks
GitHub's [webhooks](https://developer.github.com/webhooks/) feature allows you to notify any arbitrary service when something of interest happens to your repository or organisation. The possibilities this affords you are endless and exciting, so much so that we've decided to set up GitHooks.io; a platform enabling easy development, consumption and execution of webhooks.

Webhooks can automate your workflows, run your tests, perform checks on your code, and so on. But they're often bespoke, in-house solutions; written, maintained and hosted internally. A GitHook is simply a webhook which conforms to our API, and is therefore able to be executed on GitHooks.io, saving you the hassle and expense of hosting your own webhook receiver. It also provides a platform for developers to share their webhooks with the world.
