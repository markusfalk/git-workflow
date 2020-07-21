# A branching model and workflow for git

<img src="https://github.com/markusfalk/git-workflow/blob/master/git-workflow-logo.svg" alt="" height="150" width="150">

## Table of contents

* [About](#about)
  * [What are the benefits](#what-are-the-benefits)
* [Prerequisites](#prerequisites)
* [Branches](#branches)
  * [Hotfix](#hotfix)
  * [Master](#master)
  * [Development](#development)
  * [Story](#story)
  * [Release](#release)
  * [Test](#test)
* [Merge Requests](#merge-requets)
* [Naming conventions](#naming-convention)
  * [Branches](#branches-1)
  * [Tags](#tags-and-releases)
  * [Commits](#commits)
* [Best practices](#best-practices)
* [Semantic Versioning](#semantic-versioning)
* [Acknowledgments](#acknowledgments)
* [License](#license)

## About

This branching model borrows from different ideas and incorporates my experience over the years.

If you think about introducing a branching strategy to your workflow, start small and add complexity when you need it. These are all suggestions that are meant to be changed and adjusted to match your needs.

Happy Coding :nerd_face:

[Markus](https://www.markus-falk.com)

### Benefits

Having a well defined workflow has a lot of benefits. This guide aimes to provide you with all of these:

* It makes it easy to understand projects that are new to a developer
* It makes working on a project quick and efficient
* It scales well and tries to clean itself up
* It ensures all changes are tracked and nothing is lost on local machines
* It enforces many best practices for a good git hygine
* It works great witch scrum projects but is flexible enough to work in any other way
* It makes code reviews easy
* It makes setting up new projects easy
* It helpes with release management

## Prerequisites

* This strategy works under the assumption that the same source code will always produce the same compiled software
* It also assumes that you want to release all of your stories and features once they are done. I have explained an [alternative approach](#alternative-approach) to this continous method.
* This workflow is intended to be used with a centralized git workflow

```ruby
           +------+
           |      |
  +-----+  + REPO +  +-----+
  |        |      |        |
  |        +------+        |
  |            +           |
  |            |           |
  |            |           |
  |            |           |
  |            |           |
  +            +           +
USER A       USER B      USER C

     [Centralized Workflow]
```

## Branches

The figure below shows what the branching model looks like. Arrows to the right (+->) indicate where to branch off from. Arrows to the left (<-+) show where to merge back into. The small dots (•) represent tags/releases.

```ruby
  +              +              +             +            +            +
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              •<- 1.1.0 --------------------------------+            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  +----- 1.0.1 ->•------------->|             |            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              |              <-------------+            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              |              |             |            |         --->
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  <--------------+              +------------->            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  |              +-------------->             |            |            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  |       1.0.0  •              +-------------------------->            |
  |              |              |             |            |            |
  |              |              |             |            |            |
  +              +              +             +            +            +
hotfix         master      development      story        release        test

                                   [Branching Model]

```

### Hotfix

This branch is used to merge urgent fixes that cannot wait for the next planned release.

* branch from master
* merge to master and development

### Master

Master is your main branch. Tags and Releases are created in that branch. It can go live anytime and tags here are used for rollbacks if necessary.

### Development

This is the main development or integration branch. It contains all finished story and feature branches. Integration tests can be run here. It is used as a safety net so that you do not need to work or rely on master until you really have to. It can also be used to share finished stories that another story builds upon but that is not yet released.

* branch and update from master

### Story

This branch represents a user story known from agile software development methodologies. Create one of these branches for each story/feature you want to develop to seperate unfinished work from the code base.

* branch from development
* merge to development
* gets deleted after the story has been merged to master and released

### "Release"

To prepare for a new relase a new release branch is created (have a look at the [naming conventions](#naming-convention)).

In this branch you have the freedom to choose whatever bugfix or story you would like to release. If a story didn't make it in time for the release, then just do not merge it into this branch.

Because the integration of all changes needs to be tested, this branch is very likely to be identical to one of your [test branches](#test). But in this branch you can add version numbers or change logs - whatever is related to the release only.

* branches from development
* merges into master creating a tag/release
* will be deleted after the release has been rolled out

#### Alternative Approach

The previously described approach assumes that all finished stories are automatically planned for release. In case you want more control over what is being released after a sprint you can branch off the release branch from master, then merge all of your story branches into it that you want to be part of the release. Then merge this back to master.

Feature Flags could also be used instead of the release branch, they provide some benefits over the integration branch strategy like A/B-Testing but also increase your lines of code.

### "Test"

The 'test' branches are 'read only branches' that can be used to install a set of features onto a testing environment or staging server. Simply merge any branch you want to test or cherry pick commits into this branch. You can have as many testing branches as you like. These branches can be used for automated integration tests. With every push on one of them, an automated build can deploy this branch to your testing server.

* branched from any other (depending on what you want to test - could be hotfix branch, development a story or a feature)
* never merges back into any other branch

Examples:

```txt
customer-preview
pre-release
...
master
development
```

## Merge Requests

To spread knowledge about the codebase and programming practices to all developers and to ensure the quality of your code and branches I suggest to use pull/merge-requests when merging stories into development and definitly when merging anything back into master.

Let one of the approvers of your merge request be your integration system that runs all linters and tests.

## Naming Convention

To identify the different types of branches it is usefull to follow a certain naming convention. Naming commits a certain way can help developers read what has happened and could be used to generate changelogs.

### Branches

The name of a branch consists of the following delimited by '-':

* number or identifier of issue
* short description (use _ for spaces)
* name of branch author (used to identify who should clean up branches after release/merge)

Schema:

```txt
<#issue>[/<#issue>]-<short_description>-<author.name>
```

Examples:

```txt
123-a_standalone_feature-markus.falk
456-a_story-markus.falk
456/789-sub_feature_of_a_story-markus.falk
```

:information_source: When using multi repos the identifier at the start can be used to keep track of matching changes (e.g. for back-end and front-end changes in different repos for the same story).

The **exceptions** to the rule are:

* master
* development
* test branches: can be named after your test-servers (e.g.: test-integration-server, test-pre-production-server)
* release branches: the are named after their release. Just like the corresponding tag (e.g.: v1.1.2)

### Tags and Releases

After all the work is done it is time to release something. Here is what that cycle could look like:

* finished story branches get merged to development branch
* a new release branch is created from development
* after a successfully tested and approved deployment of this release branch, it can be merged to master and gets a new tag
* master branch is merged to development branch
* development branch is merged to all story or feature branches that did not make it into the last release
* all released story, bugfix or release branches will be deleted by their authors

#### Tagging

Tags should be used to record certain releases. Not only for archiving purposes but also for rolling back when something goes wrong with deployment. It is recommended to use annotated tags because the latest commit (default annotation) might not sufficiently describe the whole release.

```txt
git tag v<semver> -m <Annotation>
```

Example:

```txt
v1.0.0 'Initial Release of Awesome'
```

### Commits

There are a number of major types of commits:

* **bugfix**: the commit fixes something
* **docs**: the commit changes the documentation only
* **feature**: the commit introduces something new
* **hotfix**: the commit fixes something for a hotfix release
* **test**: the commit fixes or adds tests

* **task**: everything else

I recommend using [commitlint](https://commitlint.js.org) with the following schema:

```txt
type(scope?): subject

body?

footer?

refs <#issue>
```

Example:

```txt
feature(UserProfile): get profile image

Calling API to get image for profile view.

refs #1234
```

:information_source: I have also published [commitlint rules](https://github.com/markusfalk/commitlint-config-git-workflow) that you can install to match this guide.

#### Alternative

If you prefer a more visual approach to distinguish your commits you could try [gitmoji](https://gitmoji.carloscuesta.me). It also comes with a pre-commit hook and a cli.

#### Imperative tense

All descriptions are written in imperative tense: 'change' not 'changes' or 'changed'. A good way to remember this to think of the words "This commit will ..." and then following this write your description.

Example:

```txt
This commit will ... "feature: add ajax function refs #1234"
This commit will ... "bugfix: remove ajax function refs #1234"
```

## Best practices

* merge regularly (top to bottom: master -> development -> story)
* commit early and often
* commit related changes
* don't commit half done work
* When merging a feature back into the development branch use ```--no-ff``` to keep track of where the commit came from in your log.

## Semantic Versioning

[Semantic versioning](http://semver.org/) is used for tags and releases.

## Acknowledgments and Furter Reading

* [GIT branching guidance for devops teams](https://writeabout.net/2018/05/04/git-branching-guidance-for-devops-teams/)
* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [GIT cheat sheet](http://pixelbrackets.github.io/git_cheat_sheet/git_cheat_sheet.pdf)
* [Everything GIT at Atlassian](https://www.atlassian.com/git/)
* [Graphics created with asciiflow](http://asciiflow.com/)

## Branch by Abstraction

[![GOTO 2017 • Feature Branches and Toggles in a Post-GitHub World • Sam Newman](http://img.youtube.com/vi/lqRQYEHAtpk/0.jpg)](http://www.youtube.com/watch?v=lqRQYEHAtpk)

