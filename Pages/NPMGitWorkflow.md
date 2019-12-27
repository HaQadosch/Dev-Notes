# NPM

## Bump versions

https://docs.npmjs.com/cli/version 


# Git

https://git-scm.com/book/en/v2 

## A good commit message
https://chris.beams.io/posts/git-commit/ 

### Example

```txt
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```

## Editor

Set up your editor for git messages, so that as soon as you want to commit some code, git opens a temp file in your editor of choice and let you fill in a complete message.
```sh
  $ git config --global core.editor emacs
```
`git commit -m ""` will just invite to write a subject line.
You can though add multiple -m to add a paragraph to the commit

```sh
  git commit -m "this is the subject" -m "next paragraph" -m "and then the next"
```

## Template

The advantage of opening your editor for commit messages is that you can use a template, by setting the `commit.template` config.

You can create a `~/.gitmessage.txt` and it will be used to fill in the commit file opened by your editor.

```txt
# subject (50 chars pls) Module: Action #ticket-number
#
# Small paragraph for the WHAT
# 
# Small paragraph for the WHY
#
# --
## If this commit ends the work of the current tasks then add the Resolves
# Resolves #ticket-number
# See also: #ticket1, #ticket2
```
To tell git to use the template, set up the config
`git config --global commit.template ~/.gitmessage.txt`

## Alias

From the `~/.gitconfig` file 
```sh
[alias]
  a        = git add . && git status
  aa       = git add . && git add -u . && git status
  ac       = git add . && git commit
  acm      = git add . && git commit -m
  # alias    = git config --list | grep 'alias\.' | sed 's/alias\.\([^=]*\)=\(.*\)/\1\t => \2/' | sort
  au       = git add -u . && git status
  c        = commit
  ca       = commit --amend
  cm       = commit -m
  d        = diff
  l        = log --graph --all --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(white)- %an, %ar%Creset'
  lg       = log --color --graph --pretty=format:'%C(bold white)%h%Creset -%C(bold green)%d%Creset %s %C(bold green)(%cr)%Creset %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
  ll       = log --stat --abbrev-commit
  llg      = log --color --graph --pretty=format:'%C(bold white)%H %d%Creset%n%s%n%+b%C(bold blue)%an <%ae>%Creset %C(bold green)%cr (%ci)' --abbrev-commit
  master   = checkout master
  s        = status
```