# Parameterised GitHooks
You can specify parameters to be sent along with your GitHooks - these will be appended to the webhook URL as a GET parameter.

The current allowed parameter types are:

* url
* string
* boolean (cast to 0 or 1 in the URL)

GitHooks.io will validate `url` to make sure it is actually a URL, etc.

Example: you want a `template` parameter. You would choose type 'url', key 'template', and the resulting webhook would be something like:

http://githooks.io/githooks/YourName/YourRepo?template={USER INPUTTED VALUE}

You can then access this value in your GitHook code using `GET.template`.
