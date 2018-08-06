---
title: "Advanced Git Fu"
date: 2018-08-06
excerpt: >-
  Exploring advanced `git` commands and command flags that can get you out of
  some hairy places as a senior dev working on a decent-sized team. Working
  through a typical scenario as a team contributor, we learn some new tricks
  with `git log`, `git diff`, `git merge` and `git rebase`.
reveal:
  theme: "night"
  css: >-

    .reveal section li {
      line-height: 1.4;
      padding-bottom: 1ex;
    }
    .reveal code {
      color: purple;
    }

---
<script type="text/template">

# Advanced Git Fu
## The Learning Continues

----

{% include aside.img.html
  src="https://gravatar.com/avatar/ece1d171f07e8c58d191a938e249d885?s=700"
%}

## Who is this guy?

### David Rogers AKA _"AL the X"_

- [`linkedin.com/in/althex`](https://linkedin.com/in/althex)
- [`github.com/al-the-x`](https://github.com/al-the-x)
- [`twitter.com/al_the_x`](https://twitter.com/al_the_x)

----

### Tell 'em what you're gonna tell 'em.

1. A quick review of the commands we're all familiar with.
2. An example of how to use them (perhaps differently) day-to-day on a large project.
3. A scenario where something goes kinda bad and we fix it.
4. What happens to _other_ contributors _after_ we fix it.
5. How we could have prevented that mess in the first place.

----

## This is an _interactive_ session...

### Please interrupt. Please ask questions.

#### Save Q's about what preso tools I'm using until after...

----

{% include aside.img.html pull="right"
  src="stack-overflow-frequent-git-questions.png"
%}

### Caveat: `git` is a _command line tool_

- Slick GUIs make the most common tasks easier
  _but hide advanced bits_
- Most devs don't try learning a thing
  _until they need to know it_
- The instruction manual is hard to use, harder to search

----

### A moment about that manual...

- Check the `man` pages, Luke!
  - `git help <command>`
  - `git <command> --help`
  - `q` to "quit", `?` for help
- Quick refresher: `git <command> -h`
- Not _everything_ is in the `man` pages, though... :(
- You can't ask a `man` page a question...<br>
  ...but see https://git-scm.com/docs/ instead!

----

# Chapter 1:
## In which some familiar commands learn some new tricks...

----

{% include aside.asciicast.html
    src="git-status-add-commit.cast"
%}

### Boring everyday usage...

- `git status` -- what has changed since I last committed changes?
- `git add <path>` -- stage changes ready for commit
- `git commit` -- record staged changes

----

{% include aside.asciicast.html
    src="git-status-add-patch.cast"
%}

### Command flag awesomeness!

- `git status --short` -- less boilerplate, more signal, less noise
- `git add --patch` -- stage one "hunk" at a time interactively <br>
  try `e`(dit) mode!
- `git config --global core.editor <not-vim>`

----

{% include aside.asciicast.html
  src="git-patch-happy.cast"
  pull="right"
%}

### Get your `--patch` on!

- `git add --patch` -- add hunks to the stage
- `git checkout --patch` -- discard hunks
- `git reset --patch` -- remove hunks from the stage
- `git stash --patch` -- _stash_ selected hunks

----

{% include aside.asciicast.html
  src="git-aliases.cast"
%}

### Make your own `git` commands!

```sh
git config --set alias.<alias-name> '<alias-value>'
```

```sh
$> git config alias.st 'status --short'

$> git config alias.aliases \
  '!git config --get-regexp "alias"'

$> git config alias.branches \
  '!git for-each-ref refs/heads \
      --format="%(refname:short)"'
```

More examples: <br> https://github.com/al-the-x/dotfiles/

----

## Funny `git log` tricks!

### Who needs a GUI anyway?

----

{% include aside.asciicast.html
  src="git-log-output.cast"
  pull="left"
%}

- Display commits as a tree with <br>
  - `--oneline` -- single line output
  - `--graph` -- dots and lines
  - `--decorate` -- branch and tag names
- Include `--all` references to _taste the rainbow!_
- Quick peek at what changed in each commit with `--stat`

----

# Chapter 2

## In which we royally screw something up...

----

{% include aside.asciicast.html
  src="git-scenario-1-setup.cast"
%}

### Real-world Scenario

- I'm working on a new feature for our product.
- I created a feature branch based on `master`
- I pushed my branch to GitHub and [opened a Pull Request (PR)](https://guides.github.com/introduction/flow)
- I can't automatically merge the PR because of _merge conflicts!_

----

### Do The Most Obvious Thing.

![Advice on resolving conflicts as displayed on the GitHub Pull Request](https://help.github.com/assets/images/help/pull_requests/merge_conflict_error_on_github.png)

> I should back-merge `master` into my branch! <br> GitHub even told me so!

----

{% include aside.asciicast.html
  src="git-pull-conflict.cast"
%}

### How do I do that?

Unfortunately, `git pull origin master` into my branch generates a confusing "update on delete" conflict...

Abort! Abort!

----

{% include aside.asciicast.html
  src="git-log-left.cast"
%}

### What did _they_ do?

Check the commits in `master` that I'm missing in my branch:

- `HEAD` is short-hand for "wherever you are now"
- `--oneline` shortens the commit hash and message to _one line_
- `--stat` shows a "diffstat" of each commit

----

{% include aside.asciicast.html
  pull="right"
  src="git-log-right.cast"
%}

### What did _I_ do?

Check the commits from _my branch_ missing from `origin/master` for comparison:

- `HEAD..origin/master`
- `origin/master..HEAD`

There's my file getting _deleted!_

----

### Aside: What's with the dots?

`branch-A..branch-B` -- commits in `branch-A` that are **NOT** in `branch-B`

- shorthand version of `branch-B ^branch-A` (because shell escaping)
- omitting a branch implies `HEAD` so `branch-A..` _equals_ `branch-A..HEAD`
- `--reverse` shows the _commits_ in chronological order

#### Not to be confused with!

`branch-A...branch-B` -- commits in _either_ `branch-A` _or_ `branch-B`
  that <br>  _are not shared_... Very confusing without `--graph --left-right`, TBH.

----

## Back to the hunt!

----

{% include aside.asciicast.html
  src="git-log-filename.cast"
%}

### How can I find _that_ change?

I can use filter `git log` by filename to see changes to _that file._

- use `--full-diff` to see _other files changed_
- use `-<N> --skip <M>` to zoom in on one
- use `--patch` to see the diff for each commit

----

{% include aside.img.html
  src="https://imgs.xkcd.com/comics/regular_expressions.png"
  pull="right"
%}

#### So what if that didn't work?

I can _search_ for changes with... <br> _Regular Expressions!_

- use `-G <some-pattern>` to search for _code changes_ by pattern
- use `--full-diff` to show _all changes_ in each matched commit
- use `--patch-with-stat` to show the diff _and_ diffstat

----

{% include aside.asciicast.html
  src="git-log-pattern.cast"
%}

### By the power of _Regular Expressions!_

I discover that my missing file was _renamed across two commits!_

1. `path/to/file.ext` was _deleted_
2. `some/other/path/to/file.ext` was _added_

But they don't look completely the same, either...

----

### The power of `git diff`...

`git diff` can compare _any two objects_ you can name: files, paths, commits.
It can even compare files that aren't in source control.

- `<commit>` can be described by its short or long hash
- `<commit>^` describes the _parent_ of `<commit>`
- `<commit>~<N>` describes `<N>` commits _prior to_ `<commit>`
- `<commit>:<path>` describes the state of `<path>` _at_ `<commit>`

----
<!-- .slide: style="font-size: 90%" -->

{% include aside.asciicast.html
  src="git-diff-paths.cast"
%}

### How do I compare two files _across time?_

- `some/other/path/to/file.ext` was added in commit `abcdef123`
- `path/to/file.ext` was _deleted_ in the _previous_ commit: `abcdef123^`
- `abcdef123~2:path/to/file.ext` describes it _before the delete_
- use `-b` to ignore whitespace changes

Now we know how to integrate the changes in `master` into our branch!

----

# Chapter 3

## In which we try to fix this unholy mess...

----

## How do I _fix_ this?

1. Make _our branch_ look like `master` _before_ merging with a rename.
2. Use a different _merge strategy_... and a little rebasing.
3. Update the upstream branch in the remote... forcibly.

----

{% include aside.asciicast.html
  src="git-mv-merge.cast"
  pull="right"
%}

### The Imitation Game

- use `git mv` to relocate our file to match the location in `master`
- use `git commit` to add the relocation to our branch
- use `git merge -s resolve` to try a _different_ "merge strategy"

Check the results, though... Let's try something different.

----

{% include aside.asciicast.html
  src="git-rebase-interactive.cast"
%}

#### The case for `git rebase`...

- `git rebase --interactive` edits a list of "todos" for each commit
- rearrange the commits so the "rename only" commit is first (bottom)
- bonus: `git rebase` also accepts `--strategy` (`-s`)

Well, sometimes you have to deal with conflicts in life.

----

{% include aside.img.html
  src="http://meldmerge.org/images/meld-merge-full.png"
%}

#### `git mergetool` to the rescue!

- The default is `vimdiff` of course... Gotcha again!
- Use one of the GUIs with `--tool`; `--tool-help` lists available
  - OSX: `opendiff` via XCode
  - Linux: [`meld`](http://meldmerge.org/), `vimdiff3`!!
  - Win: [`codecompare` ($$)](https://www.devart.com/codecompare/), ...

----

# Chapter 4

## In which _everyone else_ is royally screwed and we help them fix it...

----

{% include aside.asciicast.html
  src="git-branch-diverged.cast"
%}

### What could go wrong?

- `git` tracks commits by hash _only_: different hash, different commit
- `git rebase` alters the hash of _every downstream commit_.
- `git pull` generates a big mess... even `--rebase` can't help us!

Remember: `git reset --merge` or `git rebase --abort`

----

### Time for my _FINAL FORM!!_

- `git rebase <upstream> <branch>`
  - finds where `<upstream>` and `<branch>` diverged
  - reapplies commits missing from `<upstream>` _as_ `<branch>`
  - great for "catching up" a topic branch!
- `git rebase --onto <base> <upstream> <branch>`
  - checks out `<base>`
  - re-applies `<branch>` _back to but not including_ `<upstream>`
  - perfect for resolving "branches diverged" problems!

----

### Yeah, but... Example?

{% include aside.asciicast.html
  src="git-rebase-onto.cast"
%}

- we want to apply our work `--onto origin/branch-A`
- we can use `git log` to find the range of commits to include
- we apply those commits (using shorthand) via `git rebase`
- we have to `push --force` our branch once it looks right

----

# Chapter 5

## In which we discuss how we could have avoided this all in the first place...

----

### 1. Make _ATOMIC_ commits.

- Don't rename a file AND refactor in the same commit.
- _Definitely_ don't rename a file or folder and commit the deletion and the
  addition across 2 or more commits.
- Make _minimal_ changes to a file that you move or rename.

----

### 2. Don't `merge` unless fast-forwarding.

- Remember: `git pull` is `git fetch && git merge` (unless you set `pull.rebase true`).
- Set `merge.ff only` to ensure that `git pull` only works for fast-forward (clean) merges.
- Only `pull` your _shared_ branches: `master`, `develop`, `production`

----

### 3. Don't share feature branches, if possible.

- Relying on another branch makes `rebase` scary and harder than it has to be.
- Consider separate "topic" branches that can merge into mainline cleanly.
- Talk to me if this sounds really hard for your team.

----

### 4. Communicate!

- When _considering_ refactoring, renaming, or generally rennovating, tell your
  team about it _in advance_. Ask for anyone who might also touch that code.
- Keep the refactor small and _atomic_, separate from functionality changes.

----

### Tell 'em what ya told 'em.

- `git log` is more useful than you can imagine
- `git rebase` is your friend
- keep refactors _atomic_
- `vim` is not a trap

----


</script>
