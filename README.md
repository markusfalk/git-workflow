# git-workflow
Yet another branching strategy

## Branches

<img src="https://raw.githubusercontent.com/markusfalk/git-workflow/master/branching.png" alt="Branches">

### Hotfix

This branch is used to merge in urgent fixes that cannot wait for the next planned release.

* branch from master
* merge to master and development

### Master

As usual this is your main branch. Here Tags/Releases are created.

### Development

This is the main development branch. It contains all implemented features. Integration tests can be run here.

* branch from master
* merge to master (creating a release)

### Feature

Feature branches are used to implement new features, no kidding. Create branches for small units of code. They live as long as you are working on something new.

* branch from development
* merge into development

### Release

For preparing a new release and to add minor fixes or changes release branches are created. There you can add version numbers or change logs.

* branches from development
* merges into development & master (creating a tag/release)

### Test

The test branch is a 'read only branch' that can be used to install a set of features onto a testint environment. Simply merge any branch you want to test or cherry pick commits into this branch.

* never merges back into any other branch.

## Naming Convention

### Branches

The name of a 'regular' branch consists of the following delimited by '-':

* Date
* name of branch author
* issue #
* short description (use _ for spaces)

```
YYYYMMDD-<name>-<issue#>-<description>
```

Example:

```
20151006-falk-1234-my_new_branch
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
* **Task**: everything else

To identify them easily within git log commit messages follow this schema:

```
[<CATEGORY>] <description> #refs <issue#>
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

I recommend [semantic versioning](http://semver.org/) for Tags and Releases.

Also you can find some accumulated info about this on the [front-end cheat sheet](https://github.com/markusfalk/front-end-cheatsheet/blob/master/pdf/front-end-cheat-sheet.pdf?raw=true) [PDF].

## Kudos

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [GIT cheat sheet](http://pixelbrackets.github.io/git_cheat_sheet/git_cheat_sheet.pdf)
* [Everything GIT at Atlassian](https://www.atlassian.com/git/)
