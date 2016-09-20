# GitHook Ideas
The GitHooks.io project maintainers don't have the time to create all of the wonderful GitHook possibilities out there, so if you'd like to contribute but don't know where to start, try implementing one of these!

## Active projects

### GitHook: Super Protected Branches
Rolls back any commits made directly to a branch of your choice, branching it and making a PR instead. This is to stop people from being able to push directly to `master`.

We have a proof of concept with our [githook-super-protected-branches](https://github.com/GitHooksIO/githook-super-protected-branches) repository, but it could do with some love and care.

### GitHook: Forced Code Reviews
A GitHook which unmerges any PR which was opened and merged by the same person.

We got as far as creating a [githook-forced-code-reviews](https://github.com/GitHooksIO/githook-forced-code-reviews) repository. Please donate a PR!

## New ideas

### GitHook: Open Source Cleaner
When you make your private repository public, this GitHook will clear all commit history and replace it with one commit.

### GitHook: Pull Request Cleaner
When a PR is opened, merge all commits into one.

### GitHook: Automated Tagging
Given a filename and property (e.g. `package.json` and `version`), whenever the version number changes, a tag is automatically created.

### GitHook: Label reorganiser
When a new repo is created, automatically reconfigure the GitHub Issues labels with your preferred configuration.

### GitHook: Twitter integration
By passing a Twitter handle and authentication token to a GitHook, it should be possible to automatically tweet when you tag a new release.
