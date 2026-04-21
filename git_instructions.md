# Using `git` and GitHub

`git` is a tool for keeping code NSYNC for teams of developers working across
different machines. For D3, this will primarily be our individual laptops and
servers where the code will be ran.

A *repository*, or *repo* is a directory which is tracked by `git`. In the base 
directory of a repo you'll find a .git directory that holds all of the tracked 
changes and history.

GitHub stores repositories in a single place that can be referenced by the rest 
of the team.

## Safety notice!

Git makes it easy to move a lot of files around really easily, and sometimes
that leads to files that shouldn't be moved being moved. Let's talk about how to
prevent common errors at the outset.

### Commit with intention

Only add your current changes to a commit. You may change other things to make
the project work in your environment, try to avoid committing these and only
commit changes that you intend to make to the entire project.

### Use a .gitignore

`.gitignore` files keep files you **never** want to show up in your repository
from showing up. You can use wildcards to skip whole file types if it is
helpful. '*.csv' for example will keep any csv from being committed.

(Look at an example)

## Important command reference

- `clone`: copy a remote repo to your local machine
- `status`: show changed, staged, and untracked files
- `diff`: let's you page through current changes
- `add`: stage changes for the next commit
- `commit`: save staged changes to local history
- `push`: upload local commits to the remote
- `pull`: download and merge remote commits
- `git stash`: temporarily store uncommitted work, `git stash pop` restores them
- `checkout`: switch branches or restore files, the `-b` flag adds a new branch
- `log`: view commit history
- `revert`: undo a commit by creating a new one

## Main ideas

### Sign in to GitHub with git

You can sign into GitHub with VSCode, with a ssh key pair, or with a GitHub key.

### Clone

`git clone` copies a repo from a 'remote,' typically GitHub, but it could be any
directory locally or otherwise that is a valid git repo.

### Commit changes

`git commit` takes any *added* changes and adds them to a commit, a save point
that git tracts. Commits are the core idea of git.

The `-a` flag adds all changes to *previously added* files.
The `-m` flag allows you to follow the commit command with a string message.

### Change branch

`git checkout <branch name>` checks out a current branch, or 
`git checkout -b <branch name>` creates a new branch.

Depending on the project and task, you might commit all changes to a new branch 
which will be merged to the main branch with a pull request.

### Push changes

If all teammates are working on a single branch, you can push directly to that
branch and deal with the conflicts that may arise. Otherwise if you're working
on a 'feature' branch your changes won't conflict.

#### Pull request (GitHub concept)

You changes will be added to the main branch after a review through a 'pull
request' that can be submitted with GitHub.

### Pull changes

Typically this is very simple. If no one has edited a file that you have also
edited and committed to the files with update effortlessly. There are a couple
of instances when that isn't the case.

#### Case 1. Handling the rebase prompt when pulling

When first run `git pull` and there are new commits on the remote that conflict
with your local commits, git will ask how to reconcile them:

```
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands prior to your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
```

**Our team uses merge.** Choose it by running:

```bash
git config pull.rebase false
```

To set this once and never be asked again, add `--global`:

```bash
git config --global pull.rebase false
```

After setting this, re-run `git pull`. Git will create a merge commit that
combines your local work with the incoming changes.

#### Case 2. Resolve conflicts

A conflict happens when you and a teammate both edited the same lines in the
same file. Git doesn't know which version to keep, so it marks the file and
asks you to decide.

Conflicted files will be listed by `git status`. Open one and you'll see
markers like this:

```
<<<<<<< HEAD
your version of the line
=======
their version of the line
>>>>>>> origin/main
```

To resolve it:

1. Edit the file — delete the markers and keep whichever version is correct
   (yours, theirs, or a mix of both).
2. Save the file.
3. Stage it with `git add <filename>`.
4. Repeat for any other conflicted files.
5. Once all conflicts are resolved, run `git commit` to finish the merge.

VS Code highlights conflict markers and offers **Accept Current Change** /
**Accept Incoming Change** buttons inline, which can be easier than editing
the markers by hand.

### Revert to old code

Use `git revert` to undo a previous commit without erasing history. This is
safer than deleting commits because it adds a new commit that reverses the
changes, leaving the full history intact.

1. Find the commit you want to undo with `git log`. Each commit has a hash like
   `a3f92c1` — copy it.
2. Run `git revert a3f92c1` (using your hash). Git will open an editor to
   confirm the commit message — save and close it.
3. Push the revert commit with `git push`.

Your teammates will see the revert in the history and can pull it normally.

> **Note:** `git revert` undoes one specific commit. If you want to undo
> several commits, run `git revert` once for each, starting from the most
> recent.

## Git best practices (that I often ignore)

1. Commit often
2. Push often
    - You can push almost as much as you commit.
    - Definitely push all changes when done with a working session.
3. Try to write identifiable commit messages that are relatively short, git has
   tools to identify the specifics of what changes so typically detailed
   descriptions are not needed.
