# .githook.yml
GitHooks.io looks for a YAML file called `.githook.yml` in the root of your repository. It should look something like this:

```yaml
title:       Hello World
version:     0.1.0
description: "This is my description of my Hello World application."
thumbnail:   http://path/to/thumbnail.jpg # optional. Defaults to your user avatar.

# relative to the root
entry_point: hello-world.js

# specify the language your GitHook is written in (currently only node_js is supported)
language: node_js

# require at least one event to trigger the GitHook. See 'Events' below.
triggered_on:
  - push

# optional. Add as many scopes as required. See 'Scopes' below.
# Note that 'admin:repo_hook' will always be available (since it is required by GitHooks.io to properly manage GitHook installations) - but you should still specify it if you need it, just to be safe.
scopes_required:
  - 'user:email'

# optional - see "Parameterised GitHooks" below
parameters:
  'my_parameter_name':
    title:       Name
    description: Who am I greeting?
    # options: 'string', 'url', 'boolean'. default: 'string'
    type:        string
    # options: 'yes', 'no'. default: 'no'
    required:    no

# optional
tags:
  - something
  - something-else
```
