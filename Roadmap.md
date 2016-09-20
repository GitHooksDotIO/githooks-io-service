# Roadmap

## In progress

### 'Select' GitHook parameter type
Will be able to provide users with a dropdown list of available parameter values.

### Usage statistics
Start tracking how many repositories are using a given GitHook, giving users an idea of a GitHook's popularity.

## In the pipeline

### Add support for latest Webhook events
GitHub added some new events in April: https://github.com/blog/2156-expanded-webhook-events

### Complete Webhook management
Rather than just 1-click installs, you will now be able to delete GitHooks from within GitHooks.io or edit your custom parameters. You will be able to 'own' GitHooks installed by other users in your organisation, so that the GitHook runs as you and not them.

We may offer a dedicated webhook view for any one of your given repositories, letting you view and edit both GitHooks and standard webhooks.

### Search functionality
We'll start off with very few GitHooks, but over time we'll need search functionality.

### Support for additional webhook languages
Let people write GitHooks in more than just Node.

### User scope management
GitHook users will have an admin area where they can allow or revoke granular scopes, and see what scopes they have already granted.

### Tagging
`.githook.yml` will be able to list tags, which will enable users to find and compare related GitHooks before they choose to install.
