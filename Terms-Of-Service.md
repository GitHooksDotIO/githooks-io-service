# Terms of Service

## Definitions
* `GitHooks.io` refers to the http://githooks.io website.
* `GitHook` refers to a webhook contributed to and hosted on the GitHooks website.
* `GitHooks` is the plural of `GitHook`.
* `Webhook` is a GitHub-specific term denoting an arbitrary piece of code that is executed when certain GitHub events are triggered. Here, we refer to webhook(s) in their 'native' form - they become 'GitHooks' only once they have been contributed to GitHooks.io.
* `Consuming` GitHooks is to install a GitHook to your repository.
* `Contributing` GitHooks is to contribute your webhook to GitHooks.io.

## Consuming GitHooks
By consuming a GitHook through our awesome one-click install process, you allow GitHooks.io to execute the related GitHook with whatever scope you have granted GitHooks.io to run, when the necessary event has been triggered.

Please note that if you grant additional scopes to GitHooks.io, those scopes become available to all previously installed GitHooks.

GitHooks.io can take no responsibility for any damage caused by installed GitHooks. You are encouraged to check the source code of any GitHook prior to consumption, just in case.

## Contributing GitHooks
You agree not to store user access codes provided by GitHooks.io for longer than is required for the current execution of your GitHook.

(Apart from it being untrustworthy, it's also unreliable, as the access code will change whenever a user grants or revokes scopes).

You agree also to not write GitHooks which are so badly or maliciously written that they threaten to take down the GitHooks infrastructure, nor to attempt to install and trigger GitHooks an unfair number of times in quick succession to put the servers under strain.

By contributing your webhook to GitHooks.io, you agree to let GitHooks.io host and execute your code until you revoke your webhook.

If another user contributes your repository to GitHooks.io on your behalf, we will assume permission has been implicitly granted by you if your repository is open-source and there is the presence of a `.githook.yml` file in the root.
