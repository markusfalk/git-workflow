# A branching model and workflow for <img src="https://git-scm.com/images/logos/2color-lightbg@2x.png" height="49" width="142" alt="git"/>

<img src="http://s2.quickmeme.com/img/b5/b5528b2279824b6cbb2243940570e9ba342d2fbecea732e41f8ddff3c0b9dce1.jpg" alt="">

## Table of contents

* [About](#about)
  * [What are the benefits](#what-are-the-benefits)
* [Prerequisites](#prerequisites)
* [Branches](#branches)
  * [Hotfix](#hotfix)
  * [Master](#master)
  * [Development](#development)
  * [Story](#story)
  * [Feature](#feature)
  * [Release](#release)
  * [Test](#test)
* [Naming conventions](#naming-convention)
  * [Branches](#branches-1)
  * [Tags](#tags-and-releases)
  * [Commits](#commits)
* [Best practices](#best-practices)
* [Semantic Versioning](#semantic-versioning)
* [Acknowledgments](#acknowledgments)
* [License](#license)

## About

Yet another branching strategy. It was made to be used for release-based software development projects. However, it can be adjusted to fit any type of project.

### What are the benefits

Having a well defined workflow has a lot of benefits. This guide aimes to provide you with all of these:

* It makes it easy to understand projects new to a developer
* It makes working on a project quick and efficient
* It scales well and tries to clean itself up
* It ensures all changes are tracked and nothing is lost on local machines
* It enforces many best practices for a good git hygine
* It works great witch scrum projects but is flexible enough to work in any other way
* It makes code reviews easy
* It makes setting up new projects easy
* It provides additional freedom for customization
* It makes release management easy because all features are sepereated until release. 

## Prerequisites

This workflow is intended to be used with a centralized git workflow.

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
  +              +              +             +            +            +            +
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              •<- 1.1.0 ---------------------------------------------+            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  +----- 1.0.1 ->•------------->•<------------+            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             <------------+            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |         --->
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             +------------>            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  <--------------+              +------------->            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              +-------------->             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |       1.0.0  •              +--------------------------------------->            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  +              +              +             +            +            +            +
hotfix         master      development      story       feature      release        test

                                   [Branching Model]

```
### Hotfix

This branch is used to merge in urgent fixes that cannot wait for the next planned release.

* branch from master
* merge to master and development

### Master

As usual this is your main branch. Tags/Releases are created in that branch. It can go live anytime and is used for rollbacks if necessary.

### Development

This is the main development branch. It contains all finished features. Integration tests can be run here.

* branch and update from master

### Story

If a set of features can be accumulated into something bigger or if you are working on something with other developers this branch is where it goes. This branch represents a user story known from agile software development methodologies. Smaller features and tasks can be branched from the story branch for individual development.

* branch from development
* merge to development
* gets deleted after the story has been merged to master and released

### Feature

Feature branches are used to implement new features, tasks or bugfixes. Create branches for small units of code. They live as long as you are working on them.

* branch directly from development or a story branch
* merge back into development or a story branch
* gets deleted after the feature has been merged to story/master

### Release

To prepare for a new relase a new release branche is created. There you can add version numbers or change logs - whatever is related to the release only.

* branches from development
* merges into development & master (creating a tag/release)

### Test

The 'test' branch is a 'read only branch' that can be used to install a set of features onto a testing environment/stage/server. Simply merge any branch you want to test or cherry pick commits into this branch. You can have as many testing branches as you like. I am using  a 'pre-production-server' and 'test-server' branch. Whenever a push on one of those happens it is autmatically installed on one of those machines.

* branched from any other (depending on what you want to test - could be hotfix branch, development a story or a feature)
* never merges back into any other branch!
* delete after the test is finished succesfully

## Naming Convention

To identify the different types of branches I just mentioned it is usefull to follow a certain naming convention. Naming conventions for commits are independent from the branching model but I suggest using them. They have proven to be useful in my development experience.

### Branches

The name of a branch consists of the following delimited by '-':

* Date
* types - can be hotfix, story, feature, bugfix or task (see commits for more info on those types)
* #issue
* name of branch author. (makes it easy to identify who should clean up branches after release)
* short description (use _ for spaces)

Schema:
```
YYYYMMDD-<TYPE>-<#issue>[-<TYPE>-<#issue>]-<author.name>-<short_description>
```

Example:
```
20151006-BUGFIX-111-m.falk-a_standalone_bugfix
20151006-FEATURE-123-m.falk-a_standalone_feature
20151006-STORY-456-m.falk-a_story_we_work_on
20151006-STORY-456-FEATURE-789-m.falk-subtask_of_a_story
```

The **exceptions** to the rule are:

* master
* development
* test
* Release Branches: the are named after their release. Just like the corresponding tag (v.1.1.2)

### Tags and Releases

After all the work is done it is time to release something. Here is what that cycle could look like:

* feature branches get merged to their story branches
* story branches get merged to development branch
* a new release branch is created from development
* after a successful deployment, the release branch can be merged to master and gets a new tag
* master branch is merged to development branch
* development branch is merged to all story or feature branches that did not make it into the last release
* all released story, feature, bugfix or release branches will be deleted by their authors
* celebrate, repeat :)

#### Tagging

Tags should be used to record certain releases. Not only for archiving purposes but also for rolling back when something goes wrong with deployment. It is reccomended to use annotated tags because the latest commit (default annotation) might not be sufficiently describe the whole release.

```
git tag v<semver> -m <Annotation>
```

Example:

```
v1.0.0 'Initial Release of Awesome'
```

### Commits

There are 3 major TYPES of commits:

* **FEATURE**: the commit introduces something new
* **BUGFIX**: the commit fixes something
* **HOTFIX**: the commit fixes for a hotfix release
* **TASK**: everything else

To identify them easily within git log commit messages follow this schema:

```
[<TYPE>] <description> refs <#issue>
```

Also descriptions are written in imperative tense: 'change' not 'changes' or 'changed'.

Example:

```
[FEATURE] add ajax function refs #1234
[BUGFIX] remove ajax function refs #1234
```

## Best practices

* merge regularly
* commit early and often
* commit related changes
* don't commit half done work
* When merging a feature back into the development branch use ```--no-ff``` to keep track of where the commit came from in your log.

## Semantic Versioning

[Semantic versioning](http://semver.org/) is used for tags and releases.

Also you can find some accumulated info about this on the [front-end cheat sheet](https://github.com/markusfalk/front-end-cheatsheet/blob/master/pdf/front-end-cheat-sheet.pdf?raw=true) [PDF].

## Acknowledgments

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [GIT cheat sheet](http://pixelbrackets.github.io/git_cheat_sheet/git_cheat_sheet.pdf)
* [Everything GIT at Atlassian](https://www.atlassian.com/git/)
* [Graphics created with asciiflow](http://asciiflow.com/)

## License

The MIT License (MIT)

Copyright 2016 Markus Falk
