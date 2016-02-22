# Roadmap

## In progress

### GitHook thumbnails
The site is a little text-heavy. Taking inspiration from [npmjs.com](https://www.npmjs.com/), GitHooks will let GitHook owners specify thumbnails for their GitHooks.

### Better documentation
Listing available events and user scopes.

### More GitHook parameter types
GitHooks can be parameterised, but at the moment we just have a text input field for each parameter. Some parameters ought to be checkboxes (i.e. boolean) or select/option inputs (i.e. limited choice), although of course in the URL these will all be converted to string.

### Usage statistics
Start tracking how many repositories are using a given GitHook, giving users an idea of a GitHook's popularity.

## In the pipeline

### GitHook: Forced Code Reviews
A GitHook which unmerges any PR which was opened and merged by the same person.

### GitHook: Open Source Cleaner
When you make your private repository public, this GitHook will clear all commit history and replace it with one commit.

### GitHook: Pull Request Cleaner
When a PR is opened, merge all commits into one.

### GitHook: Delete Merged Branches
When a PR is merged, the merged branch is automatically deleted.

### GitHook: Automated Tagging
Given a filename and property (e.g. `package.json` and `version`), whenever the version number changes, a tag is automatically created.

### Complete Webhook management
Rather than just 1-click installs, you will now be able to delete GitHooks from within GitHooks.io or edit your custom parameters. You will be able to 'own' GitHooks installed by other users in your organisation, so that the GitHook runs as you and not them.

We may offer a dedicated webhook view for any one of your given repositories, letting you view and edit both GitHooks and standard webhooks.

### Search functionality
We'll start off with very few GitHooks, but over time we'll need search functionality.

### Support for additional webhook languages
Let people write GitHooks in more than just Node.

### Tagging
`.githook.yml` will be able to list tags, which will enable users to find and compare related GitHooks before they choose to install.
