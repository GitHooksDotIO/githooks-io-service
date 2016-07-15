# Roadmap

## In progress

### Versioned GitHook installs
Using GitHub tags.

### Usage statistics
Start tracking how many repositories are using a given GitHook, giving users an idea of a GitHook's popularity.

## In the pipeline

### Add support for latest Webhook events
GitHub added some new events in April: https://github.com/blog/2156-expanded-webhook-events

### 'Select' GitHook parameter type
Will be able to provide users with a dropdown list of available parameter values.

### GitHook: Forced Code Reviews
A GitHook which unmerges any PR which was opened and merged by the same person.

### GitHook: Open Source Cleaner
When you make your private repository public, this GitHook will clear all commit history and replace it with one commit.

### GitHook: Pull Request Cleaner
When a PR is opened, merge all commits into one.

### GitHook: Automated Tagging
Given a filename and property (e.g. `package.json` and `version`), whenever the version number changes, a tag is automatically created.

### GitHook: Label reorganiser
When a new repo is created, automatically reconfigure the GitHub Issues labels with your preferred configuration.

### Complete Webhook management
Rather than just 1-click installs, you will now be able to delete GitHooks from within GitHooks.io or edit your custom parameters. You will be able to 'own' GitHooks installed by other users in your organisation, so that the GitHook runs as you and not them.

We may offer a dedicated webhook view for any one of your given repositories, letting you view and edit both GitHooks and standard webhooks.

### GitHook: Twitter integration
By passing a Twitter handle and authentication token to a GitHook, it should be possible to automatically tweet when you tag a new release.

### Search functionality
We'll start off with very few GitHooks, but over time we'll need search functionality.

### Support for additional webhook languages
Let people write GitHooks in more than just Node.

### User scope management
GitHook users will have an admin area where they can allow or revoke granular scopes, and see what scopes they have already granted.

### Tagging
`.githook.yml` will be able to list tags, which will enable users to find and compare related GitHooks before they choose to install.
