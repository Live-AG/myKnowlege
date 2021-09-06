
# Git Install

`sudo apt update`

`sudo apt install git`

`cd ~/Documents`

`git clone https://github.com/Live-AG/myKnowlege`

---

# VS code install

	sudo snap install --classic code # or code-insiders

# Git configuration

    git config --global user.email "AGusarov@live.ag"
    git config --global user.name "Andrey Gusarov"

---

# Git commit

`git add -A`

`git commit -a -m "new PS"`

# Git push

`git push --all`

#Push to other storage

    git pull --rebase <path>


-----------------------------

# Pushing to Multiple Git Repos

If a project has to have multiple git repos (e.g. Bitbucket and
Github) then it's better that they remain in sync.

Usually this would involve pushing each branch to each repo in turn,
but actually Git allows pushing to multiple repos in one go.

If in doubt about what git is doing when you run these commands, just
edit ``.git/config`` (`git-config(1)`_) and see what it's put there.

# Remotes

Suppose your git remotes are set up like this::

    git remote add github git@github.com:muccg/my-project.git
    git remote add bb git@bitbucket.org:ccgmurdoch/my-project.git

The ``origin`` remote probably points to one of these URLs.


# Remote Push URLs

To set up the push URLs do this::

    git remote set-url --add --push origin git@github.com:muccg/my-project.git
    git remote set-url --add --push origin git@bitbucket.org:ccgmurdoch/my-project.git

example

        git remote set-url --add --push origin https://github.com/live-ag/myKnowlege
        git remote set-url --add --push origin http://AGusarov:gFKrZ6G9LWJXAxQ2ubcu@localhost:9999/root/myknowlege.git


It will change the ``remote.origin.pushurl`` config entry. Now pushes
will send to both of these destinations, rather than the fetch URL.

Check it out by running::

    git remote show origin


# Per-branch

A branch can push and pull from separate remotes. This might be useful
in rare circumstances such as maintaining a fork with customizations
to the upstream repo. If your branch follows ``github`` by default::

    git branch --set-upstream-to=github next_release

(That command changed ``branch.next_release.remote``.)

Then git allows branches to have multiple ``branch.<name>.pushRemote``
entries. You must edit the ``.git/config`` file to set them.


# Pull Multiple


You can't pull from multiple remotes at once, but you can fetch from
all of them::

    git fetch --all

Note that fetching won't update your current branch (that's why
``git-pull`` exists), so you have to merge -- fast-forward or
otherwise.

For example, this will octopus merge the branches if the remotes got
out of sync::

    git merge github/next_release bb/next_release

# Push to a repository using access_token

    git remote add github git@github.com:muccg/my-project.git
    git remote add github git@bitbucket.org:ccgmurdoch/my-project.git

    git remote add origin https://<access-token-name>:<access-token>@gitlab.com/myuser/myrepo.git

examle

    git remote add gitlab http://AGusarov:gFKrZ6G9LWJXAxQ2ubcu@localhost:9999/root/myknowlege.git

Token   `gFKrZ6G9LWJXAxQ2ubcu`