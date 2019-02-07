
Anshul Choudhary [7:07 PM]
I tried to mitigate that issue of rebasing by git fetch and git pull. On my local it always says it's up to date but on my remote I see it's some commits behind and some ahead

Katrina Brock [7:08 PM]
up to date with what though?

Anshul Choudhary [7:08 PM]
origin/master
ohh it should be remote right ?

# Overall Rubrics:
the branch that you have in your PR should be
< all the commits in mainline/master > + one commit with your change
if your changes is small like this
if you do it right, on this page: https://github.mv.usa.alcatel.com/anschoud/nuage-topos/tree/nuageinfra_PR
instead of `This branch is 5 commits ahead, 38 commits behind pygash:master.`, you will see `This branch is 1 commit ahead of pygash:master`

# So what you need to do is:
- make sure your `master` branch is the same as pygash:master
- rebase your PR branch against your master branch
- push (you will have to force push)
At this point, it will say `This branch is 5 commits ahead of pygash:master.` Then
- squash your 5 commits together
- push


# Stepwise Explanation

Katrina Brock [7:16 PM]
ok, first do this:
```(devenv) ~/pygash-org/nuage-topos (INF-3572|✔)
❯❯❯ git checkout master 
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
(devenv) ~/pygash-org/nuage-topos (master|✔)
 ❯❯❯ git remote -v
kid     git@github.mv.usa.alcatel.com:corentih/nuage-topos.git (fetch)
kid     git@github.mv.usa.alcatel.com:corentih/nuage-topos.git (push)
me      git@github.mv.usa.alcatel.com:kbrock/nuage-topos.git (fetch)
me      git@github.mv.usa.alcatel.com:kbrock/nuage-topos.git (push)
origin  git@github.mv.usa.alcatel.com:pygash/nuage-topos.git (fetch)
origin  git@github.mv.usa.alcatel.com:pygash/nuage-topos.git (push)
(devenv) ~/pygash-org/nuage-topos (master|✔)
❯❯❯ git status```
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working tree clean
so from above...you can see in my repo `origin` is the name I have for for `pygash/nauge-topos` and `me` is the name I have for my fork `kbrock/nuage-topos`

[root@mvdclinuxvm26 nuage-topos]# git remote -v
origin git@github.mv.usa.alcatel.com:anschoud/nuage-topos.git (fetch)

Katrina Brock [7:17 PM]
^ok, here is your first problem, * you don't even have the upstream as one of your remotes *
you only have your fork, so you have no possible way to pull in new changes that other people have added !

Katrina Brock [7:19 PM]
then add your fork as a second remote
I'll add your fork too...
```(devenv) ~/pygash-org/nuage-topos (master|✔)
 ❯❯❯ git remote add anschoud git@github.mv.usa.alcatel.com:anschoud/nuage-topos.git
(devenv) ~/pygash-org/nuage-topos (master|✔)
 ❯❯❯ git remote -v
anschoud        git@github.mv.usa.alcatel.com:anschoud/nuage-topos.git (fetch)
anschoud        git@github.mv.usa.alcatel.com:anschoud/nuage-topos.git (push)
kid     git@github.mv.usa.alcatel.com:corentih/nuage-topos.git (fetch)
kid     git@github.mv.usa.alcatel.com:corentih/nuage-topos.git (push)
me      git@github.mv.usa.alcatel.com:kbrock/nuage-topos.git (fetch)
me      git@github.mv.usa.alcatel.com:kbrock/nuage-topos.git (push)
origin  git@github.mv.usa.alcatel.com:pygash/nuage-topos.git (fetch)
origin  git@github.mv.usa.alcatel.com:pygash/nuage-topos.git (push)```
* ^ this is how you add another remote*
see, your fork is in the list now
I've named it `anschoud`
you can name it whatever you like locally..some people call it `fork`



ok, now...you probably want your `master` branch to track mainline, not your fork
so run `git branch --set-upstream-to=mainline/master`
then run `git status` again and show me the output
oh wait, you actually have 4 commits in your master branch...do you want to save those? https://github.mv.usa.alcatel.com/anschoud/nuage-topos/tree/master

Anshul Choudhary [7:25 PM]
yeah
also, I got his error
Untitled 
git branch --set-upstream-to mainline/master
error: the requested upstream branch 'mainline/master' does not exist
hint:
hint: If you are planning on basing your work on an upstream
hint: branch that already exists at the remote, you may need to
Untitled 
git branch --set-upstream-to=mainline/master
error: the requested upstream branch 'mainline/master' does not exist
hint:
hint: If you are planning on basing your work on an upstream
hint: branch that already exists at the remote, you may need to

Katrina Brock [7:26 PM]
ok, before you do that, let's save the changes you have in master in another branch

Anshul Choudhary [7:26 PM]
okay

Katrina Brock [7:26 PM]
what do you want to name it?
(doesn't much matter)

Anshul Choudhary [7:27 PM]
just `changes`

Katrina Brock [7:27 PM]
ok, so do `git branch changes` < - this creates a new branch (edited)
then `git checkout changes` < - this makes the new branch your working branch (edited)

Anshul Choudhary [7:28 PM]
done

Katrina Brock [7:28 PM]
ok, so now you have the branch locally, you probably want to push it to your remote in case your workstation goes down or something
so do `git push origin`

Anshul Choudhary [7:29 PM]
done

Katrina Brock [7:30 PM]
great, that worked: https://github.mv.usa.alcatel.com/anschoud/nuage-topos/tree/changes
we can come back to this branch later, but let's move forward with cleaning up and fixing your existing PR
so now...go back to your local `master` branch

Anshul Choudhary [7:30 PM]
git checkout master

Katrina Brock [7:31 PM]
yes!
now...the thing is when you do `git remote add` you tell git what you want to call a remote repo
but it doesn't actually contact that remote repo until you tell it to

Anshul Choudhary [7:31 PM]
yeah I named it mainline right

Katrina Brock [7:31 PM]
so that's probably why you got the error above
it doesn't know that that `mainline` has any such branch as master
well...it doesn't known anything about `mainline` at all except where to find it
so to solve this run `git fetch mainline`
that will pull all the info from your remote, but it doesn't change any of your local branches

Anshul Choudhary [7:32 PM]
done, it did

Katrina Brock [7:33 PM]
now try `git branch --set-upstream-to=mainline/master`
see if that solves it

Anshul Choudhary [7:33 PM]
Untitled 
git push --set-upstream origin changes
Branch changes set up to track remote branch changes from origin.
Everything up-to-date

Katrina Brock [7:34 PM]
ok, so your `changes` branch is tracking anschoud/nuage-topos:changes. that's ok

Anshul Choudhary [7:39 PM]
so, now git fetch and pull ? (edited)

Katrina Brock [7:50 PM]
sorry, someone walked up to talk to me
back now

Anshul Choudhary [7:50 PM]
no probs
I am in lecture anyways :stuck_out_tongue:

Katrina Brock [7:51 PM]
so your `changes` branch is tracking anschoud/nuage-topos:changes. But! what you really want is your `master` branch to be tracking pygash/nutopos:master
so that you have latest changes locally
so do `git checkout master; git branch --set-upstream-to=mainline/master`
then `git status`
if it works, you should then `git status` should tell you that you're 4 commits ahead 38 commits behind mainline/master

Anshul Choudhary [7:52 PM]
Untitled 
git rebase origin/nuageinfra_PR
Current branch nuageinfra_PR is up to date.
[root@mvdclinuxvm26 nuage-topos]# git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.

Katrina Brock [7:53 PM]
```Your branch and 'mainline/master' have diverged,

and have 4 and 38 different commits each, respectively.```
good!
```Untracked files:

  (use "git add <file>..." to include in what will be committed)



        nuage-topos/```
it seems you cloned the repo inside itself
so, maybe rm -rf that?

Anshul Choudhary [7:54 PM]
Untitled 
git status
On branch master
Your branch and 'mainline/master' have diverged,
and have 4 and 38 different commits each, respectively.
 (use "git pull" to merge the remote branch into yours)

Katrina Brock [7:55 PM]
cool
is everything making sense?

Anshul Choudhary [7:56 PM]
yeah

Katrina Brock [7:56 PM]
I'm telling you what commands to type, but I hope you're understanding too
if you don't understand what you're doing, please ask questions

Anshul Choudhary [7:57 PM]
but I was thinking in the last part why do I need to keep my mster branch in line
I mean I am raising my PR from the nuageinfra_PR
so why not just go there and fetch
unless it fetches from the mainline which I have as my remote

Katrina Brock [7:58 PM]
so here's what has happened...
before you touched this repo...there was just a bunch of commits
pygash/nutopos (remote) A - B - C - D
then you forked
so then it's like
```pygash/nuage-topos:master (remote) A - B - C - D
anschoud/nuage-topos:master (remote) A - B -C D```
then you clone, so you have
```pygash/nuage-topos:master (remote) A - B - C - D
anschoud/nuage-topos:master (remote) A - B - C - D
local:master A - B - C - D```
then you branched so you have
```pygash/nuage-topos:master (remote) A - B - C - D
anschoud/nuage-topos:master (remote) A - B - C - D
local:master A - B - C - D
local:PR A - B - C - D```
(edited)
then add a commit, then push
```pygash/nuage-topos:master (remote) A - B - C - D
anschoud/nuage-topos:master (remote) A - B - C - D
local:master A - B - C - D
local:PR A - B - C - D - YOURCHANGES
anschoud/nuage-topos:PR (remote) A - B - C - D - YOURCHANGES```
(edited)
now...that's all fine..but the problem is..in the meantime, people have added to mainline
so we have
```pygash/nuage-topos:master (remote) A - B - C - D - E - F - G

anschoud/nuage-topos:master (remote) A - B - C - D

local:master A - B - C - D

local:PR A - B - C - D - YOURCHANGES

anschoud/nuage-topos:PR (remote) A - B - C - D - YOURCHANGES```
so we want to know if your changes have any problems before we merge
but the problem is.... E - F - G are now required for the code to work
so when we test your branch, even if you changes are harmless, it's likely to fail
because it's missing E - F - G
so as it is, there's no way to know if your changes are harmless
so the goal is to get to
```anschoud/nuage-topos:PR (remote) A - B - C - D - E - F - G - YOURCHANGES```
to get there, you first get your master to match the mainline master
```local:master A - B - C - D - E - F - G```
then we rebase against it so you get
```local:PR A - B - C - D - E - F -G  - YOURCHANGES```
now, techically you're right, you can just rebase mainline directly
to do that, you checkout your PR and just do `git rebase mainline master`
I think it's easier to keep everything clean in your head if you have a local branch that is always up to date with the mainline master
it just helps stay organized
hopefully this isn't too basic, I just want to make sure we're 100% on the same page

Anshul Choudhary [8:14 PM]
so, master in my local is just a dummy I am keeping to which I match my other branch if I am pushing changes through them or if it's just the master then I will just keep it updated by git fetch && pull remote master...
and then push my changes
so, if I checkout to my brach say changes and do git fetch and pull remote mainline....it will update my changes branch but I still have to update my master to the mainline (just to eep it clean incase I raise a change through it in future)

Katrina Brock [8:16 PM]
local master should match mainline remote master
that's just a good practice, it's not the absolutely only way to do it
then you branch off of it and make changes on other branches
the `changes` branch or the `PR` branch or what have you
but those don't get automatically updated from master
you have to rebase for that

Anshul Choudhary [8:19 PM]
and rebase would direct me to that mainlineremote branch . From there on I will still have to do fetch and pull correct ?

Katrina Brock [8:21 PM]
mmm..not really
ok, so first for all `pull` = `fetch` + `merge`
so "fetch and pull" doesn't make much sense
what `fetch` does is take all the changes in your remote (whichever one you're fetching either origin or mailnine) and stores them locally
it doesn't change your local branches at all
for understanding merge and rebase, you should just read this: https://www.atlassian.com/git/tutorials/merging-vs-rebasing
Atlassian
Merging vs. Rebasing | Atlassian Git Tutorial
Compare git rebase with the related git merge command and identify all of the potential opportunities to incorporate rebasing into the typical Git workflow

Anshul Choudhary [8:24 PM]
okay

Katrina Brock [8:25 PM]
it will take too long for me to explain without a shared whitboard, the diagrams should help

Anshul Choudhary [8:25 PM]
cool

Katrina Brock [8:26 PM]
for this PR, it's not such a big deal..but like..if want to do anything with devops here or at most any software company these are some concepts that really good to know, so worthwhile to spend the time to learn them now.

Anshul Choudhary [8:26 PM]
yeah,  I will read that
I did git pull and push

Katrina Brock [8:27 PM]
yeah, you have to understand what those are doing though
like I said `pull` is `fetch` + `merge`
so to really understand `pull` you have to understand `merge`

Anshul Choudhary [8:28 PM]
pull was for fetching the changes others have made
push is whatever I did

Katrina Brock [8:28 PM]
yes, but exactly what will happen to those other commits?
say the situation is this
```pygash/nuage-topos:master (remote) A - B - C - D - E - F - G
local:master A - B - C - D - X - Y```
and you `git pull`
what will be the result in `local:master` ?
ABDCDEFGXY? or ABCDXYEFG? or something else?
these kind of things, it helps to understand
so when i goes wrong, you understand why

Anshul Choudhary [8:31 PM]
you have already done the rebasing here right
and then doing git pull

Katrina Brock [8:32 PM]
>pull was for fetching the changes others have made
even this is not quite right...well...it will only fetch other changes if your branch is set to track the mainline
if your branch is set to track your own remote
like...your fork
you will not pull in any new changes with `git pull`

Anshul Choudhary [8:32 PM]
A-B-C-D-X-Y_E-F-G
correct, so if I am on my local first step would be to do git remote origin/master and then so git pull ?
this will fetch changes from the origin or mainline (which we are calling)
right

Katrina Brock [8:35 PM]
so, what I think you should do at this point is do `git checkout master; git reset --hard mainline/master;`
that will make it so that your local master exactly matches mainline master
this is actually kind of a trick question
Katrina Brock
ABDCDEFGXY? or ABCDXYEFG? or something else?
Direct MessageYesterday at 8:30 PM
it doesn't do either of these
(by default)
by default, it makes a "merge commit"
so it's ABCXYZ
where Z is the child of both G and Y
https://git-scm.com/docs/git-merge

Anshul Choudhary [8:39 PM]
just the head ? E-F ?

Katrina Brock [8:39 PM]
...?

Anshul Choudhary [8:39 PM]
you said Z is child of G and Y.....what about E-F ?
they are also changes which are being committed and not in local

Katrina Brock [8:40 PM]
Z will have all the changes from E and F as well (edited)
if there are conflicts between XY and EFG, you have to resolve them

Anshul Choudhary [8:41 PM]
cool
Untitled 
git reset --hard mainline/master
HEAD is now at 892ebd5 Merge branch 'master' of github.mv.usa.alcatel.com:pygash/nuage-topos
[root@mvdclinuxvm26 nuage-topos]# git status
On branch master
Your branch is up-to-date with 'mainline/master'.
for my remote fork I have to repeat the process to be updated
like this one is for mainline

Katrina Brock [8:44 PM]
to update your remote fork, you just do `git push -f origin master`

Anshul Choudhary [8:45 PM]
okay, so I do everything with my master
then update my fork from there
whcih fetches the latest commits

Katrina Brock [8:45 PM]
you should be taking changes from mainline and putting them in your remote fork
there's rarely a reason to pull from you remote fork
only reason would be if you get a new workstation or something...
and you need *your* changes
but mostly you will be taking other peoples changes from remote
so you will get them from the mainline remote
anyway, I need to sign off...

Anshul Choudhary [8:47 PM]
so in this process of `git push -f origin master` all my branches will be updated to the latest commit of the mainline ?

Katrina Brock [8:47 PM]
no
just the one you have checked out

Anshul Choudhary [8:47 PM]
aha

Katrina Brock [8:47 PM]
will get pushed to the master branch of your fork

Anshul Choudhary [8:47 PM]
and I have to do for every branch

Katrina Brock [8:48 PM]
only if you make changes to the branch

Anshul Choudhary [8:48 PM]
I mean the commits from other users anyways have to be pulled right

Katrina Brock [8:48 PM]
yeah
then you push them to your remote
let's follow up tomorrow


Anshul Choudhary [8:50 PM]
Sure, thank you
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short


Can we do 8:30 instead of 8? 12:30 your time
Er... Sorry 11:30

Katrina Brock [11:26 AM]
are you around? are we doing a weekly update?

Katrina Brock [11:39 AM]
btw - I looked at your history on your linux vm and found where you pushed to mainline master (edited)
  ```734  git checkout nuageinfra_PR
  735  git status
  740  git status
  741  git branch --set-upstream-to=mainline/master
  742  git status
  743  git pull
  744  git status
  745  git branch -a
  746  git push```
`---set upstream` sets the default where you pull and push
and remember pull does NOT mean "make my branch exactly like the remote branch"
pull means `fetch` + `merge`

Anshul Choudhary [1:46 PM]
so that means I should have only done `git fetch` at that moment to avoid this ?

Katrina Brock [1:46 PM]
no. you should not have done `git push`
because you are pushing your local changes to the remote branch that you have set as `upstream`
so because you have set upstream to mainline/master
it pushed your changes to mainline
if you did `git push origin/master`
then it would have pushed to master branch on your remote
but I think what you wanted was `git push origin/nuageinfra_PR` (edited)
which would have pushed the changes to your PR branch

Anshul Choudhary [1:50 PM]
right...I was asking because when I am doing `git pull` then I am fetching the changes from the remote to my local and then, as you said `pull`=`fetch`+`merge`. Here, `merge` to what , am I not only pulling whatever is there on that remote branch (mainline) ?

Katrina Brock [1:51 PM]
even if you just did `git fetch` and `git push`, you would still be pushing your changes to mainline
that's the problem

Anshul Choudhary [1:52 PM]
isn't that odd....I am trying to get something from remote and at the same time my changes are being pushed ?

Katrina Brock [1:56 PM]
no
`git branch --set-upstream-to=mainline/master` -> you are telling git, this local branch (nuageinfra_PR) should be associated with the remote mainline/master
 `git push` -> you are telling git...push my local changes to the remote branch associated with this local branch

Anshul Choudhary [2:01 PM]
this is okay, I am asking when I do `git pull` after I add this remote upstream, I am trying to fetch whatever is there on that remote upstream to my local branch right ? and my question is when i am fetching those changes, to which branch am I pushing , since `pull`=`fetch` (which i did for remote uostream) + `push` (where is this happening). If you are saying that the `push` is happening to my remote forked(nuageinfra_PR) then, why do I see `nuageinfra` in the mainline remote branch(which I didn't want to commit) ?

Katrina Brock [2:07 PM]
so if you do `git fetch` it just takes all the commits in the remote repo and downloads them to git's database
it doesn't touch your local branches
`git pull` does that
plus eqivalent of `git merge <branch that your local branch is tracking>`

Katrina Brock [2:08 PM]
so it merges the changes from upstream into your local branch
so it merges the changes from upstream into your local branch


53 replies
Anshul Choudhary [1 hour ago]
okay,so this merge is that those changes are merged to my local. and when I did `git push` from my local I pushed those changes to my forked remote right ?


Katrina Brock [1 hour ago]
sure..but the problem wasn't pushing the upstream changes...in fact there were none afaik


Katrina Brock [1 hour ago]
and any changes in the upstream master, would already be in the upstream master


Katrina Brock [1 hour ago]
so pushing those would have no impact


Katrina Brock [1 hour ago]
the problem was you pushed your *local* changes


Katrina Brock [1 hour ago]
to the upstream master


Katrina Brock [1 hour ago]
instead of to your PR branch


Katrina Brock [1 hour ago]
so they were basically deployed with no review whatsoever

Katrina Brock [1 hour ago]
most devs would not even have permissions to do this

Anshul Choudhary [1 hour ago]
gotcha

Katrina Brock [1 hour ago]
you have permissions because you have to have owner permissions to change the github hooks

Anshul Choudhary [1 hour ago]
734  git checkout nuageinfra_PR
 735  git status
 740  git status
 741  git branch --set-upstream-to=mainline/master
 742  git status
 743  git pull
 744  git status
 745  git branch -a
 746  git push

Anshul Choudhary [1 hour ago]
in this I was on my branch

Anshul Choudhary [1 hour ago]
nuageinfra_PR

Katrina Brock [1 hour ago]
yes...the local branch is named nuageinfra_PR

Katrina Brock [1 hour ago]
but you did not associate with your origin/nuageinfra_PR as a remote branch

Katrina Brock [1 hour ago]
you associated it with mainline/master as the remote branch

Katrina Brock [1 hour ago]
 `git branch --set-upstream-to=mainline/master`

Katrina Brock [1 hour ago]
^ you are telling your local git "if I do git pull or git push, I want to pull from or *push to* the remote branch  called master in mainline repo"

Katrina Brock [1 hour ago]
if you had done `git branch --set-upstream-to=origin/nuageinfra_PR`

Katrina Brock [1 hour ago]
then `git push`

Katrina Brock [1 hour ago]
it would push to your PR branch

Anshul Choudhary [1 hour ago]
ohh

Katrina Brock [1 hour ago]
the local branch and the remote branch it's associated with don't have to have the same name

Anshul Choudhary [1 hour ago]
I thought if I am on the nuageinfra_PR on my local then my changes are pushed there but yeah now it amkes sense

Anshul Choudhary [1 hour ago]
so, every time I have to push I need to check my upstream

Katrina Brock [1 hour ago]
really safest is to be explicit

Katrina Brock [1 hour ago]
don't just do `git push`

Katrina Brock [1 hour ago]
do `git push origin/<your branch>`

Katrina Brock [1 hour ago]
then you know for 100% sure where your changes are getting pushed to

Katrina Brock [1 hour ago]
`git push` with no remote specified is a shortcut.

Anshul Choudhary [1 hour ago]
okay so suppose I set `git branch --set-upstream-to=mainline/master`

Anshul Choudhary [1 hour ago]
but I pushed changes `git push origin/nuageinfra_PR`

Anshul Choudhary [1 hour ago]
will i get back to my set mainline/master now ?

Anshul Choudhary [1 hour ago]
basically in my 4 th step now if I do git push, it will push to maineline/master right eventhough in between I pushed changes explicitly to my forked one.

Katrina Brock [1 hour ago]
if you do the two above, your changes will be pushed to origin/nuageinfra_PR, but the remote associated with that branch willl not change

Katrina Brock [1 hour ago]
so if you do `git push` at a later time, it will push to mainline/master

Anshul Choudhary [1 hour ago]
right....cool

Katrina Brock [1 hour ago]
in `git status` it tells you what remote branch you're tracking

Katrina Brock [1 hour ago]
like `Your branch is up-to-date with 'origin/master'.`

Katrina Brock [1 hour ago]
or it may say you're ahead or behind or both

Katrina Brock [1 hour ago]
also `git branch -vvv` will show you

Anshul Choudhary [1 hour ago]
so if I am ahead that menas my changes are yet to be pushed

Katrina Brock [1 hour ago]
correct

Katrina Brock [1 hour ago]
it means you have some local changes that are not there in the remote

Katrina Brock [1 hour ago]
but if the branch you're tracking is mainline/master, that's probably what you want

Anshul Choudhary [1 hour ago]
and if I am behind than I will git pull ?

Anshul Choudhary [1 hour ago]
keeping my remote as origin/nuageinfra_PR

Katrina Brock [1 hour ago]
so....this is my preferred workflow....

Katrina Brock [1 hour ago]
use `master` to track upstream master

Katrina Brock [1 hour ago]
never commit to this branch

Katrina Brock [1 hour ago]
only pull

Katrina Brock [1 hour ago]
then when you make a PR, have that one track *your remote*


Katrina Brock [2:08 PM]
but I do'nt think there were any new changes...not sure..

Katrina Brock [2:32 PM]
I hate threads in DMs...I'm moving to here...
ok...so this is my workflow
use local master to track upstream master and never commit to that branch
so `git checkout master; git branch --set-upstream-to=mainline/master; git pull`
then when you want to make changes...
have that branch set to track your remote
so `git checkout -b my-feature; git branch --set-upstream-to=origin/my-feature`
then you make some commits....
now...how do you get new changes on upstream master to your my-feature branch
1. pull them to your local master `git checkout master; git pull;` (edited)
2. rebase your local feature branch against your local master `git checkout my-feature; git rebase master;` (edited)
3.  force push to your remote `git checkout my-feature; git push -f` = `git push -f origin/my-feature` (edited)
after step 1. when you do `git status` it should say
```On branch master
Your branch is up-to-date with 'mainline/master'.
nothing to commit, working tree clean```
if it doesn't say that and it says anything else, don't proceed
after step 2. git status should say something like

On branch my-feature
Your branch is X commits behind and Y commits ahead of origin/my-feature
got it?

Anshul Choudhary [2:42 PM]
okay, so initially when you `git checkout master; git branch --set-upstream-to=mainline/master; git pull` , haven't you already pulled
why are you doing it again in step 1 ?
isn't my master already updated

Katrina Brock [2:43 PM]
unless someone added some commits to the remote since you last updated your local branch
which is the whole point
if someone merges/pushes something to mainline/master
nothing happens on your local machine
so you have to explicitly tell it to take those changes

Anshul Choudhary [2:44 PM]
ohh so, `git checkout master; git branch --set-upstream-to=mainline/master; git pull` is like one time
rest from step 1 is what everytime I will be doing
hence, git pull

Katrina Brock [2:44 PM]
`git branch --set-upstream-to=mainline/master` is onetime
git pull will only update whatever branch you have checked out
so you have to do `git checkout master; git pull;` every time you want that branch to be updated

Anshul Choudhary [2:54 PM]
and just to be sure, `git rebase` is for if there any conflicts that are to be resolved while the head of the feature branch is updated right ?

Katrina Brock [2:55 PM]
not exactly
did you read the article I sent you about it?
this one: https://www.atlassian.com/git/tutorials/merging-vs-rebasing (edited)

Anshul Choudhary [2:58 PM]
Didn't that article says the same thing , to get  to that latest feature tip

Katrina Brock [2:59 PM]
sure...but you have resolve conflicts either way
if you do merge or rebase
but basically if you have
master: ABCDE
feature: ABCXY
then after rebase you have
master: ABCDE
feature: ABCDEXY

Anshul Choudhary [3:07 PM]
right, so if those are resolved right now then , once I raise a PR I would not get the resolve conflict option or is that totally different from this one ?

Katrina Brock [3:09 PM]
correct, if you resolve the conflicts locally, you will not have any conflicts to resolve with the github GUI

Anshul Choudhary [3:10 PM]
okay, so if I miss to rebase than I might have to resolve conflicts on the GUI ?

Katrina Brock [3:11 PM]
yes
it's also a problem if you don't rebase because the test run against only the commits in your PR
(if there are CI checks)
so if there is some bad combination between commits that have been added to mainline and the commits you added to the PR, the tests will not catch that if your PR is not rebased

like....say you are using a function in your PR
and that function gets removed or renamed in mainline
then when you merged, the code will fail

Anshul Choudhary [3:15 PM]
right..... and while I am running the tests on the nuage-tools it's taking forever to execute the tests....from yesterday it's like getting stuck at a certain point.

http://thomas-cokelaer.info/blog/2015/02/git-error-cannot-do-a-partial-commit-during-a-merge/
https://stackoverflow.com/questions/26376832/why-does-git-say-pull-is-not-possible-because-you-have-unmerged-files

git reset --hard mainline/master
Git remote add <repo name> <git link of repo>
Git fetch mainline (because no one knows about the mainline)
git branch --set-upstream-to=mainline/master

git branch --set-upstream-to=origin/master
Git add <file>
Git commit <file>
Git push


vca-naster will be both on origin and upstream
to get latest changes
you'll have to git checkout vca-master
git pull  upstream vca-master
git push origin vca-master
git checkout <branch-name>
git rebase vca-master
this way your origin vca-master will be latest and your branch will have the latest changes
you can push the branch changes after rebase for the PR to be updated
you might hvae to use the -f option hile doing so
git push origin <branch-name> -f
Why we doing rebase, isn't it updated if we do the first 2 steps ?

nope
your local branch will nopt get the changes that vca-master has got recently
rebase is to avoid conflicts that might arise with changes introduced in vca-master during the time you were working on your branch

```git pull upstream vca-5.3``` 
 ```git push origin vca-5.3``` 
 ```git checkout -b <branch_name>``` 
then make your changes

Anshul Choudhary [2:46 PM]
thanks
one more thing, so in a case where i am trying to pull changes from a new branch, first I fetch it ?

Samuel Choppara [2:47 PM]
yeah

Anshul Choudhary [2:47 PM]
That ways , I can checkout to that branch

Samuel Choppara [2:47 PM]
fetch is to get a newly made branches starting point
then you can check
yup , you are right

Anshul Choudhary [2:47 PM]
which otherwise won't be present
cool
but doing git fetch upstream (haven't I already done that initially ), shouldn't I be more specific here, like `git fetch upstream vca-5.3`

Samuel Choppara [2:50 PM]
git fetch upstream gets you all branches
it just keeps your local repo up to date

Anshul Choudhary [2:51 PM]
but fetching is just like it's still in the air OR my local will  have the new changes from that upstream

Samuel Choppara [2:51 PM]
preferably
you fetch and then pull
fetch gets the starting point of a new branch
pull gets the latest update on that branch once you are on it and pull

Anshul Choudhary [2:52 PM]
correct, I guess pull will ensure that my local has the changes, pull is basically fetch+merge my changes

Samuel Choppara [2:52 PM]
yup

Anshul Choudhary [2:59 PM]
I did fetch and then tried to checkout to vca-5.3 ( did not found)

Samuel Choppara [2:59 PM]
git fetch upstream ?

Anshul Choudhary [3:00 PM]
yeah

Samuel Choppara [3:00 PM]
check if vca-5.3 is present under
git branch -a

Anshul Choudhary [3:00 PM]
yes

Samuel Choppara [3:00 PM]
what does it read?
remotes/upstream/vca-5.3
?

Anshul Choudhary [3:01 PM]
yes

Samuel Choppara [3:01 PM]
in that case git checkout upstream/vca-5.3
git checkout -b vca-5.3
the do a git pull upstream vca-5.3


Anshul Choudhary [3:03 PM]
conflicts
with vca-5.3 as well



[Solved] Git/Github Peer’s certificate issuer has been marked as not trusted by the user
devops

LisaLisa Mathew
Jul '18
“Peer’s certificate issuer has been marked as not trusted by the user” error would come when you try to use git URL with a self-signed certificate.
For this, you need to set SSL verify to false in your git config. You can do this in two ways.
Set SSL Verify to false only for specific repo:
git config http.sslVerify false


Set SSL Verify to false Globally:
git config --global http.sslVerify false


Sudarshan:-
```git remote update sudarshan; git checkout -b <new-branch> remotes/sudarshan/vtm_vrs_conv```
then
```git submodule init; git submodule update; git submodule sync;```
then to build first time:
```cd ovs; make clean; sudo -E make distclean; make mostlyclean-compile; sudo -E ./boot.sh; sudo -E ./configure --enable-Werror --with-vendor=vrs --enable-nsgvrs --prefix=/usr --sysconfdir=/etc --localstatedir=/var CFLAGS="-g -O0 `xml2-config --cflags`" LIBS="-lrt -lm `xml2-config --libs` -lpthread -lanl -ljemalloc" LDFLAGS=-L/usr/global/tools/ovs/nsg/perfmon/libnavl/libnavl-4.5.0.61;sudo -E make```
(edited)
to build subsequent times:
```cd ovs; make```
Deleting local branches in Git
$ git branch -d feature/login
Deleting remote branches in Git
git push origin --delete the_remote_branch









