<a id="toc" rel="bookmark"></a>
# Table of Contents

* [Introduction](#intro)
* [Workflow](#workflow)
* [Merging Standards](#merging-standards)
* [Naming](#naming)

<hr />

<a id="intro" rel="bookmark"></a>
# Introduction

The initial draft of this was created based on an excellent StackOverflow answer, "[git branch naming best practices](http://stackoverflow.com/a/6065944/171217)".

## Similar guides

Here are some in which I've taken inspiration from and deserve attribution:

* [https://github.com/agis-/git-style-guide](https://github.com/agis-/git-style-guide)
* [https://github.com/thoughtbot/guides/tree/master/protocol/git](https://github.com/thoughtbot/guides/tree/master/protocol/git)

## Purpose
This is by no means the authoritative document. This is another potpurri of opinionated views on how one should use or maintain git.

My intention and purpose is for the community to grow and discuss best practices germane to git.  

This guide is intended to help newcomers understand git quickly and quickly adopt and try some best practices.

[Back to the top](#toc)
<hr />

<a id="workflow" rel="bookmark"></a>
# Workflow

In projects in which patches are numerous and quickly generated, it's prudent to utilize a proper workflow. In doing so it'll prevent git from becoming an impediment upon the team. Vincent Driessen in 2010 wrote [a seminal article](http://nvie.com/posts/a-successful-git-branching-model/) titled, "A successful Git branching model", and described what now know as *git-flow model*.

## Gitflow priniciples in a nutshell

* Maintain two branches: `master` and `develop`
* Create temporary throw-away feature branches
* The master branch is most stable branch
* Maintain a `develop` branch where feature branch patches are integrated
* Tag only stable versions in the master branch

[Back to the top](#toc)
<hr />
# Merging Standards
<a id="merging-standards"></a>

## Keep your git history linear

When merging git has a proclivity to create a multi-dimensional history of what is created or amended. It is ideal to keep your git history linear.

*Not Recommended:*
```
* Merge branch 'dev' of \var\foo\ into dev
| \
|  \    4e223   More Tweaks
|  * |  3f091   Bug fix foo.
*  | /  4e123   Tweak
| /   a1234   Another commit
*  bc234   Initial commit
```

*Recommended:*

```
* 4e223   More Tweaks
* 3f091   Bug fix foo.
* 4e123   Tweak
* a1234   Another commit
* bc234   Initial commit
```

### Option A: When merging create a commit object

When doing merge requests it is best to utilize the `--no-ff` argument. 
```
$ git merge --no-ff dev
```
This forces the creation of a commit object. This allows the author and it's reviewers to see each individual commit.

### Option B: Rebase your branch before merging it back in

Rebasing allows you to place your deltas onto an existing branch you're merging in.

[Back to the top](#toc)
<hr />

<a id="naming"></a>
# Naming

## Branch naming conventions

* Use grouping tokens (words) at the beginning of your branch names.
* Define and use short lead tokens to differentiate branches in a way that is meaningful to your workflow.
* Use slashes to separate parts of your branch names.
* Do not use bare numbers as leading parts. 
* Avoid long descriptive names for long-lived branches.
* Group tokens

### Grouping

Use "grouping" tokens in front of branch names. The groups can be named appropriately to match one's workflow.

```
group1/foo
group2/foo
group1/bar
group2/bar
group3/bar
group1/baz
```

## Short well-defined tokens

Use short nouns. Choose short tokens so they do not add too much noise to every one of your branch names. 

**Examples:**

|Prefix   | Use Case                                              |
|:-------:|-------------------------------------------------------|
| wip     | Works in progress; stuff I know won't be finished soon|
| feature | Feature I'm adding or expanding                       |
| bug     | Bug fix or experiment                                 |
| junk    | Throwaway branch created to experiment                |


Each of these tokens can be used to tell you to which part of your workflow each branch belongs.

Suppose you have different stages for each change you call 'new', 'testing' and 'verified'. You can name your branches with abbreviated versions of these tags, always spelled the same way, to both group them and to remind you which stage you're in.

```
new/frabnotz
new/foo
new/bar
test/foo
test/frabnotz
ver/foo
```

*Rationale: One can quickly tell which branches have reached each different stage, and you can group them together easily using Git's pattern matching options.*

*For example:*

```
$ git branch --list "test/*"
test/foo
test/frabnotz

$ git branch --list "*/foo"
new/foo
test/foo
ver/foo

$ gitk --branches="*/foo"
Use slashes to separate parts
```

### Branch name delimiters

* Use any delimiter you like in branch names. However, slashes tend to be the most flexible. You might prefer to use dashes or dots. But slashes let you do some branch renaming when pushing or fetching to/from a remote.

```
$ git push origin 'refs/heads/feature/*:refs/heads/phord/feat/*'
$ git push origin 'refs/heads/bug/*:refs/heads/review/bugfix/*'
```

Slashes also work better for tab expansion (or command completion) in a shell. This allows easy searching with the <kbd>TAB</kbd> key. Zsh then gives me a list of branches which match the part of the token I have typed. This works for preceding tokens as well as embedded ones.

```
$ git checkout new<TAB>
Menu:  new/frabnotz   new/foo   new/bar
```

```
$ git checkout foo<TAB>
Menu:  new/foo   test/foo   ver/foo
```

zshell is very configurable about command completion and I could also configure it to handle dashes, underscores or dots the same way. 
It also lets you search for branches in many git commands, like the following:

```
git branch --list "feature/*"
git log --graph --oneline --decorate --branches="feature/*" 
gitk --branches="feature/*" 
```

*Caveat: Slashes can cause problems. Because branches are implemented as paths, you cannot have a branch named "foo" and another branch named "foo/bar". This can be confusing for new users.*

### Do not use bare numbers

Do not use use bare numbers (or hex numbers) as part of your branch naming scheme. Inside tab-expansion of a reference name, git may decide that a number is part of a sha-1 instead of a branch name. For example, an issue tracker uses names and bugs with decimal numbers. I name my related branches `CRnnnnn` rather than just `nnnnn` to avoid confusion.

```
$ git checkout CR15032<TAB>
Menu:   fix/CR15032    test/CR15032
```

If one tried to expand just 15032, git would be unsure whether one wanted to search SHA-1's or branch names, and one's choices would be somewhat limited.

## Avoid long descriptive names

Long branch names can be very helpful when looking at a list of branches. However, it can get in the way when looking at decorated one-line logs as the branch names can eat up most of the single line and abbreviate the visible part of the log.

On the other hand long branch names can be more helpful in "merge commits" if you do not habitually rewrite them by hand. The default merge commit message is Merge branch 'branch-name'. You may find it more helpful to have merge messages show up as Merge branch `'fix/CR15032/crash-when-unformatted-disk-inserted'` instead of just Merge branch `'fix/CR15032'`.

[Back to the top](#toc)
<hr />
<a id="versioning"></a>
# Versioning


Utilize [semantic versioning](http://semver.org/) for your releases.

*Why?*
* Semantic versioning allows consistency and understanding what type of releases are being produced.

Utilize tagging where appropriate. Tags should be used to mark the specific commit used to produce distributed components.  For example some external system which requires traceability of distributed products for code signing or license verification may demand to know which commit was used to produce the distribution.

*Why?*
* Development among larger teams usually means instability of code. It's important to tag the source of every distributed build result so problems can be understood and included features or bugs can be accurately reported in the future.
