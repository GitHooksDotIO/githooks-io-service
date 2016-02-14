# Roadmap

## Coming soon

### GitHook contribution
It should be possible to submit your own GitHook-compatible webhooks to the system.
Projects will require a configuration file, and they will have to be in dedicated open-source repositories.

### Detailed documentation
...for users and contributors.

### Complete Webhook management
Rather than just 1-click installs, you will now be able to see all installed webhooks for a given repository. If they're hosted on GitHooks.io, we will keep track of which events/permissions are required and update your repository's usage of that webhook automatically.

You will also be able to add/edit/delete GitHook webhooks easily, and also view non-GitHook webhooks, with perhaps some limited interaction there.

### Usage statistics
Start tracking how many repositories are using a given GitHook, giving users an idea of a GitHook's popularity.

### GitHook: Super Protected Branches
A GitHook which rolls back any commits made directly to master and/or a branch of your choice, automatically branching it and making a pull request instead.

### GitHook: Forced Code Reviews
A GitHook which unmerges any PR which was opened and merged by the same person.

### GitHook: Open Source Cleaner
When you make your private repository public, this GitHook will clear all commit history and replace it with one commit.

### GitHook: Pull Request Cleaner
When a PR is opened, merge all commits into one.

### GitHook: Delete Merged Branches
When a PR is merged, the merged branch is automatically deleted.

### More GitHook parameter types
GitHooks can be parameterised, but at the moment we just have a text input field for each parameter. Some parameters ought to be checkboxes (i.e. boolean) or select/option inputs (i.e. limited choice), although of course in the URL these will all be converted to string.

## In the pipeline

### Search functionality
We'll start off with very few GitHooks, but over time we'll need search functionality.

### Support for additional webhook languages
Let people write GitHooks in more than just Node.

### Tagging
`.githook.yml` will be able to list tags, which will enable users to find and compare related GitHooks before they choose to install.
