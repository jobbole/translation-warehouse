# git log – the Good Parts

If you’re managing a complex git codebase with multiple developers, then you may well be using a tool like GitHub or BitBucket to delve into the history and figure out branch and merge issues.

These GUIs are great for providing a nice user interface for managing pull requests and simple histories and the like, but when the workflow SHTF there’s no substitute for using `git log` and its relatively little-known flags to really dig into the situation.

You’re going to run through this with me so that I know you’ve got it. Type the commands **in bold** to follow.

This is based on material from my book [Learn Git the Hard Way](https://leanpub.com/learngitthehardway), a free sample available [here](https://leanpub.com/learngitthehardway).

## An Example Git Repository

Run this to download a fairly typical git repository that I work on:

```shell
$ git clone https://github.com/ianmiell/cookbook-openshift3-frozen
$ cd cookbook-openshift3-frozen
```

*NB this is a copy of the original repo, ‘frozen’ here to provide stable output.* 

 

## `git log`

`git log` is the vanilla log command you are probably already familiar with:

```shell
$ git log

commit f40f8813d7fb1ab9f47aa19a27099c9e1836ed4f 
Author: Ian Miell <ian.miell@gmail.com>
Date: Sat Mar 24 12:00:23 2018 +0000

pip

commit 14df2f39d40c43f9b9915226bc8455c8b27e841b
Author: Ian Miell <ian.miell@gmail.com>
Date: Sat Mar 24 11:55:18 2018 +0000

ignore

commit 5d42c78c30e9caff953b42362de29748c1a2a350
Author: Ian Miell <ian.miell@gmail.com>
Date: Sat Mar 24 09:43:45 2018 +0000

latest
```

It outputs 5+ lines per commit, with date, author commit message and id. It goes in reverse time order, which makes sense for most cases, as you are mostly interested in what happened recently.

*NOTE: output can vary depending on version, aliases,**and whether you are outputting to a terminal!My version here was 2.7.4.*

## `--oneline`

Most of the time I don’t care about the author or the date, so in order that I can see more per screen, I use `--oneline` to only show the commit id and comment per-commit.

```shell
$ git log --oneline
ecab26a JENKINSFILE: Upgrade from 1.3 only
886111a JENKINSFILE: default is master if not a multi-branch Jenkins build
9816651 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
bf36cf5 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
```

## 

## `--decorate`

You might want more information than that, though, like which branch was that commit on? Where are the tags?

The `--decorate` flag provides this.

```shell
$ git log --oneline --decorate
ecab26a (HEAD -> master, origin/master, origin/HEAD) JENKINSFILE: Upgrade from 1.3 only
886111a JENKINSFILE: default is master if not a multi-branch Jenkins build
9816651 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
```

More recent versions of git put this in the terminal by default, so things are improving for my fingers.

(Remember that your version might do `--decorate` by default fir `git log` when output goes to the terminal instead of a file).

## `--all`

```shell
$ git log --oneline --decorate --all
ecab26a (HEAD -> master, origin/master, origin/HEAD) JENKINSFILE: Upgrade from 1.3 only
886111a JENKINSFILE: default is master if not a multi-branch Jenkins build
9816651 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
[...]
a1eceaf DOCS: Known issue added to upgrade docs
774a816 (origin/first_etcd, first_etcd) first_etcd
7bbe328 first_etcd check
654f8e1 (origin/iptables_fix, iptables_fix) retry added to iptables to prevent race conditions with iptables updates
e1ee997 Merge branch 'development'
```

Can you see what it does? If you can’t, compare it to `--oneline` above and dig around to figure it out.

That’s great, but what would be great is a visual representation of all those branches…

## `--graph`

`--graph` gives you that visual representation, but in the terminal. While it might not look as slick as some git GUIs, it does have the benefit of being consistently viewed anywhere, and much more configurable to your specific needs.

And when you’re trying to piece together what happened on a 15-team project that doesn’t rebase, it can be essential…

```shell
$ git log --oneline --decorate --all --graph
* ecab26a (HEAD -> master, origin/master, origin/HEAD) JENKINSFILE: Upgrade from 1.3 only
* 886111a JENKINSFILE: default is master if not a multi-branch Jenkins build
* 9816651 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
|\ 
| * bf36cf5 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
| |\ 
| | * 313c03a JENKINSFILE: quick mode is INFO level only
| | * 340a8f2 JENKINSFILES: divided up into separate jobs
| | * 79e82bc JENKINSFILE: upgrades-specific Jenkinsfile added
| * | dce4c71 Add logic for additional FW for master (When not a node)
* | | d21351c Update utils/atomic
|/ / 
* | 3bd51ba Fix issue with ETCD
* | b87091a Add missing FW for HTTPD
|/ 
* a29df49 Missing (s)
* 51dff3a Fix rubocop
```

**DON’T PANIC!**

The above can be hard for the newcomer to parse, and there is little out there to guide you, but a few tips here can make it much easier to read.

The `*` indicates that there is a commit on the line, and the details of the commit (here the commit id, and first line of the comment) are on the right hand side.

The lines and position of the `*` indicate the lineage (or parentage) of each change. So, to take these three lines for example:

```shell
| * bf36cf5 Merge branch 'master' of github.com:IshentRas/cookbook-openshift3
| |\ 
| | * 313c03a JENKINSFILE: quick mode is INFO level only
```

The green pipes indicate that while the two changes listed here were going on, another branch had a gap between its two changes (9816651 and d21351c).

The blue line takes you to one parent of the bf36cf5 merge (what’s the commit id of the blue parent?), and the pink one goes to the other parent commit (313c03a).

It’s worth taking a bit of time to figure out what’s going on here, as it will pay dividends in a crisis later…

------

**\*If you like this post, you’ll like my book Learn Git the Hard Way***

**\*It covers all this and much more in a similar style.***

[![learngitthehardway](https://zwischenzugs.files.wordpress.com/2018/03/learngitthehardway.png?w=188&h=300)](https://leanpub.com/learngitthehardway)

------

### 

## `--simplify-by-decoration`

If you’re looking at the whole history of a project and want to get a feel for its shape before diving in, you may want to see only the significant points of change (ie the lines affected by `-–decorate` above).

These remove any commit that wasn’t tagged, branched (ie there’s no reference). The root commit is always there too.

```shell
$ git log --oneline --decorate --all --graph --simplify-by-decoration
* ecab26a (HEAD -> master, origin/master, origin/HEAD) JENKINSFILE: Upgrade from 1.3 only
| * 774a816 (origin/first_etcd) first_etcd
|/ 
| * 654f8e1 (origin/iptables_fix) retry added to iptables to prevent race conditions with iptables updates
|/ 
* 652b1ff (origin/new-logic-upgrade) Fix issue iwith kitchen and remove sensitive output
* ed226f7 First commit
```

Try tagging a specific commit not listed above, and then re-run the command.

## File Info

Using `--oneline` can be a bit sparse, so `--stat` can give you useful information about what changed.

The number indicates the numbers of lines that were changed, with insertions represented by a `+` sign, and deletions by a `-`. There’s no concept of a ‘change’ to a line as such: the old line is deleted, and then the new one added even if only one character changed.

```shell
$ git log --oneline --decorate --all --graph --stat
* ecab26a (HEAD -> master, origin/master, origin/HEAD) JENKINSFILE: Upgrade from 1.3 only
| Jenkinsfile.upgrades | 2 +-
| 1 file changed, 1 insertion(+), 1 deletion(-)
* 886111a JENKINSFILE: default is master if not a multi-branch Jenkins build
| Jenkinsfile.full | 2 +-
| Jenkinsfile.upgrades | 2 +-
| 2 files changed, 2 insertions(+), 2 deletions(-)
```

If you find `--stat` hard to remember, then an alternative is to use `--name-only`, but with that you lose the information about numbers of changes to files.

## Regex on Commits

This one’s also *really* handy. The `-G` flag allows you to search for all commits and only return commits and their files whose changes include that regexp.

This one, for example, looks for changes that contain the text `chef-client`

```shell
$ git log -G 'chef-client' --graph --oneline --stat
...
* 22c2b1b Fix script for deploying origin
| scripts/origin_deploy.sh | 65 ++++++++++++-----------------------------------------------------
| 1 file changed, 12 insertions(+), 53 deletions(-)
... 
| * | 1a112bf - Move origin_deploy.sh in scripts folder - Enable HTTPD at startup
| | | origin_deploy.sh | 148 ----------------------------------------------------------------------------------------------------------------------------------------------------
| | | scripts/origin_deploy.sh | 148 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| | | 2 files changed, 148 insertions(+), 148 deletions(-)
... 
| * | 9bb795d - Add MIT LICENCE model - Add script to auto deploy origin instance
|/ / 
| | origin_deploy.sh | 93 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| | 1 file changed, 93 insertions(+)
```

If you’ve ever spent ages searching through `git log --patch` output looking for a specific change this is a godsend…

The eccentrically-named `--pickaxe-all` gives you information about *all* files that changed in the commit, rather than just the ones that matched the regexp in the commit.

```shell
$ git log -G 'chef-client' --graph --oneline --stat --pickaxe-all
```

Try it out!