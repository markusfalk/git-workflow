# git-workflow

Yet another branching strategy.

I am using this with a centralized git workflow. Would be interesting to see how people use this with forking workflow?!

```
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

      Centralized workflow
```

## Branches

```
  +              +              +             +            +            +            +
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              •<- 1.1.0 ---------------------------------------------+            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  +----- 1.0.1 ->•------------->•<------------+            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             <------------+            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |         --->
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
  |              |              |             |            |            |            |
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
  |              |              +--------------------------------------->            |
  |       1.0.0  •              |             |            |            |            |
  |              |              |             |            |            |            |
  +              +              +             +            +            +            +
hotfix         master      development      story       feature      release        test

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

If a set of features can be accumulated into something bigger or if you are working on something new with other developers this branch is where it goes. This branch represents a user story known from agile software development methodologies. Smaller features and tasks can be branched from the story branch.

* branch from development
* merge to development

### Feature

Feature branches are used to implement new features. Create branches for small units of code. They live as long as you are working on something new.

* branch from development or story branch
* merge into development or story branch

### Release

To prepare for a new relase a new release branche is created. There you can add version numbers or change logs - whatever is related to the release only.

* branches from development
* merges into development & master (creating a tag/release)

### Test

The test branch is a 'read only branch' that can be used to install a set of features onto a testint environment. Simply merge any branch you want to test or cherry pick commits into this branch.

* never merges back into any other branch.

## Naming Convention

### Branches

The name of a 'regular' branch consists of the following delimited by '-':

* Date
* is this branch a hotfix?
* name of branch author
* #issue
* short description (use _ for spaces)

```
YYYYMMDD-[hotfix]-<name>-<#issue>-<description>
```

Example:

```
20151006-m.falk-1234-my_new_branch
```

The exceptions to the rule are:

* master
* development
* test
* Release Branches: the are named after their release. much like the corresponding tag

### Tags

```
v<semver>
```

Example:

```
v1.0.0
```

### Commits

There are 3 major categories a commit can fall into:

* **Feature**: the commit introduces something new
* **Bugfix**: the commit fixes something
* **Hotfix**: the commit fixes for a hotfix release
* **Task**: everything else

To identify them easily within git log commit messages follow this schema:

```
[<CATEGORY>] <description> #refs <#issue>
```

Also descriptions are written in imperative tense: 'change' not 'changes' or 'changed'.

Example:

```
[FEATURE] add ajax function refs #1234
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

## Kudos

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [GIT cheat sheet](http://pixelbrackets.github.io/git_cheat_sheet/git_cheat_sheet.pdf)
* [Everything GIT at Atlassian](https://www.atlassian.com/git/)
