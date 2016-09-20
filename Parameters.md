# Parameterised GitHooks
If GitHooks are useful, parameterised GitHooks are incredible.

For instance, take the [githook-slack](https://github.com/GitHooksIO/githook-slack) GitHook which posts a message to a Slack channel when something interesting happens to the repository. If it always posted to the #updates channel in Slack, it would be useful to somebody, but not everybody. What if you want a message to be posted to #git instead? With parameters, you can empower the user to use your GitHook the way they want to.

Parameters can be added to your `.githook.yml` like so:

```yaml
parameters:
  'my_parameter_name':
    title:       Name
    description: Who am I greeting?
    type:        string                # options: 'string', 'url', 'number', 'boolean', 'select'. default: 'string'
    required:    true                  # options: 'true', 'false'. default: 'false'
  satisfaction:
    title:       How satisfied are you?
    description: Rate our service.
    options:
      - Very satisfied
      - Somewhat satisfied
      - Dissatisfied
```

## Parameter attributes
Every parameter MUST have a `title` and `description`. The title is the name of the field when it is displayed to the user.

You can provide either a `type` property (if omitted, defaults to `string` - see 'Parameter types' below) or `options` property (see 'Select type').

You can also mark a parameter as required by explicitly setting the `required` property to `true` (it defaults to `false`). The user will be unable to install the GitHook unless they provide a value for that parameter.

## Parameter types
Parameters can be one of the following types:

* url
* string
* boolean
* number
* _special case: 'select'_ (see below)

GitHooks.io knows to cast the correct type when it is passed to your module, so you can be sure that `data.parameters.myCustomBoolean` will only ever be `true` or `false`.

Depending on the type you choose for your parameter, it will determine how your parameter gets rendered to the user on install. A `string` type is presented as a normal `input` box, a `boolean` is a checkbox, and so on.

### 'Select' type
If you want to present the user with a finite list of options, you can display a dropdown by omitting the `type` property and instead setting an `options` property, whose value is an array of all the possible values the user might select.

The value at the top of the array will be the default.

