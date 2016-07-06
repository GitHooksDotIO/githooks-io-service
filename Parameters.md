# Parameterised GitHooks
You can specify parameters to be sent along with your GitHooks - these will be appended to the webhook URL as a GET parameter.

The current allowed parameter types are:

* url
* string
* boolean (cast to 0 or 1 in the URL)
* number

GitHooks.io will validate `url` to make sure it is actually a URL, etc.

Example: you want a `template` parameter. You would choose type 'url', key 'template', and the resulting webhook would be something like:

http://githooks.io/githooks/YourName/YourRepo?template={USER INPUTTED VALUE}

You can then access this value in your GitHook code using `GET.template`.

You can make a parameter required by setting `required: true`.

## Parameters in the .githook.yml

Example:

```yaml
parameters:
  'my_parameter_name':
    title:       Name
    description: Who am I greeting?
    type:        string                # options: 'string', 'url', 'number', 'boolean', 'select'. default: 'string'
    required:    no                    # options: 'yes', 'no'. default: 'no'
```
