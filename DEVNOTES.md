# DEVNOTES 
## ChrisWeekly.com `blog` repo

Repo for TBD CWX blog, under 'ChrisWeekly.com' GH org

This started as `../dotcom/README.md`.
Given leveraging gatsby-starter-egghead-blog, keeping its (Gatsby-derived) README, and DEVNOTES serves diff purpose.
Use of dotcom repo per se = TBD.

20200414:
```
npm i -g gatsby-cli
gatsby new hello
cd hello
gatsby develop
blogdev: http://localhost:8000
GraphiQL: http://localhost:8000/__graphiql
```

20200504:
```
dotcom
git co dev
n -> 13.14.0 (with npm 6.14.4)
npmlsg
  gatsby-cli@2.12.8
  npm@6.14.4
```

## FIRST-TIME GATSBY UX TRAINWRECK - OVERVIEW
tried gatsby --version, and it complained (hard stop, errors) bc not latest version.
ok, so try updating gatsby per instructions, npm i -g gatsby-cli, blows up on missing peerdeps? >/
wtf, errors and missing peer deps... npm still sucks. 
Trust yourself, Chris! you like and know yarn. and global installs are almost always a mistake.
Sure enough, the egghead starter uses yarn:
  https://github.com/eggheadio/gatsby-starter-egghead-blog
  ... tho they invoke npm 1x here https://github.com/eggheadio/gatsby-starter-egghead-blog/blob/master/package.json#L72 
The Gatsby docs are a hot mess. ppl opening issues to complain (rightly) abt broken step-by-step instructions for the simplest 1st experience getting started.
And the response is "yes, you're expected to dive headlong into dependency hell".
No. We can do much better.
Gatsby has a fair amount of complexity, and it's at least arguably justified.
But this onboarding UX is just abysmal, and it doesn't need to be.


## NODE VERSIONS: `n` not `nvm`

Gatsby TUT RECS: `brew install node`, then install and use `nvm`.

CW PREFERENCE / ALT:  `brew install n`, then use `n` for ALL node version mgmt.
// I used nvm for a couple years and was glad to have a way to manage node versions, though annoyed by its shell invasiveness.
// Then I learned about n, started using it, and never looked back. Simpler, cleaner, better.

## PACKAGE MGR: `yarn`, not `npm`
// TODO write abt why this pref. Note pnpm alt too.

## GLOBAL INSTALLS: `npx gatsby foo`, not `npm i -g gatsby-cli` && `gatsby foo`
Gatsby docs suggest a global install of gatsby-cli. 
But global installations are a bad idea on general principle.
Especially when invoking `gatsby` triggers errors and exits if you don't have the latest version!
^ ?? Doesn't this script behavior imply that npx almost has to be used? Every time it's invoked, it's required to update to the latest version.
And given an `npm global` context, this means any peer deps must also be globally installed? wat.
That is just a trainwreck.

If you follow the instructions to update it -- `npm i -g gatsby-cli` -- that in turn will blow up on missing peer deps.
This is arguably npm's fault, not gatsby's, but a user following Gatsby's official instructions will absolutely get derailed here.

## GATSBY NEW vs CLONE STARTER, W GIT UPSTREAM CONN MAINTAINED
... then using `gatsby new` to scaffold using arbitrary starters.

Alt: to start working with the gatsby-starter-egghead-blog starter -- which thankfully uses yarn, not npm, need 
instead of `gatsby new` (via global install of gatsby-cli) I think this implies simply cloning the repo of the starter I chose.
Note once I've installed the starter, will still need to invoke gatsby (ie, the gatsby-cli binary). can use npx.
Options for cloning as a temp bootstrap mech vs keeping a connection to the upstream:
(A) clone to another dir, rm the .git subdir, mv the contents to new dotcom/blog dir
(B) fork it (no harm; maintain conn to upstream in case I ever care), then pull it down from my own repo, and 1-time `git upstream` config step.

## tangent - test n -> node -> npm binaries' versions and fs loc
```
dotcom
n -> 13.14.0 (with npm 6.14.4)
// try n 12 -- to confirm which npm I'd invoke... 
n 12
n -> 12.16.3 (still 6.14.4)
n 10 -> 10.20.1 ("")
n 8 -> 8.17.0 (npm 6.13.4)
npm --version 6.13.4 === yep, good, the npm is coming strictly from what's node-provided... which is 100% from n only. perfect.
the /usr/local/lib/ (ie, `npmlsg`) is this same good version, nothing more to do here wrt cleanup. yarn ho!
```

-----------------------------------------------------------------------


## pivot ---------------------------------------------------------------
## from dotcom repo to forked blog repo ##

```
n 13
cd ~/repos/github/ChrisWeekly/
git clone git@github.com:eggheadio/gatsby-starter-egghead-blog.git blog
cd blog
yarn
// 13 warnings (not errors) abt unmet peerdeps... but finished, Done in 55.17s.
gatsby develop
// zsh cmd not found -- oh, right, even tho this repo spec's a dep on gatsby-cli^2.8.5, it doesn't provide it. ok, npx it is.
npx gatsby develop
...
  You can now view gatsby-starter-egghead-blog in the browser.
    http://localhost:8000/  ⠀
  View GraphiQL, an in-browser IDE, to explore your site's data and schema
    http://localhost:8000/___graphql  ⠀
  Note that the development build is not optimized.
  To create a production build, use gatsby build⠀
  success Building development bundle - 17.589s
  17 pages
```

## IDEA - BLOG POST
this process was a PITA and relied on plenty of prior knowledge. Ppl are suffering bc of this.
I could **write up even just the steps I've done here so far**, and it'd provide value. always be writing!

## FTW: local dev env is running, and I can start writing in the blog anytime. 11:21a
√ rename gs-egghead-blog -> blog
√ mv dotcom README to blog/DEVNOTES.md (peer of upstream's README, diff purposes)

## tangent: fix my mistake wrt git upstream vs origin 
whoops! mis-handled origin vs upstream w my fork.
intent: having forked from upstream (egghead), preserve the upstream relationship.
but work w my fork as origin.

I like PR workflow; did the 1st commit in a `dev` br, then did
`git push -u origin dev`...

but that failed bc local dev br had no upstream set. so, did:
```
  $ git push --set-upstream origin dev
  Enumerating objects: 6, done.
  Counting objects: 100% (6/6), done.
  Delta compression using up to 8 threads.
  Compressing objects: 100% (4/4), done.
  Writing objects: 100% (4/4), 2.35 KiB | 2.35 MiB/s, done.
  Total 4 (delta 2), reused 0 (delta 0)
  remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
  remote:
  remote: Create a pull request for 'dev' on GitHub by visiting:
  remote:      https://github.com/ChrisWeekly/gatsby-starter-egghead-blog/pull/new/dev
  remote:
  To github.com:ChrisWeekly/gatsby-starter-egghead-blog.git
  * [new branch]      dev -> dev
  Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```
... which seems 100% correct.

but on github, in trying to create the PR via GUI, the target br was the egghead repo not my fork. Unsure if I was careless, or if PRs are bound to upstream in latest gui? Upd: Yep, I was just careless. next time just watch the GUI dropdown more carefully, be sure the PR diff is against my fork not the upstream.

√ close the PR and delete the egghead repo's "ChrisWeekly/dev" br. (can't delete the PR, oh well.)

Anyway, I had neglected to be explicit in setting the upstream; doing that now:
`git remote add upstream git://github.com/eggheadio/gatsby-starter-egghead-blog.git`

... and it's all 100% sorted.
at some point I may want to pull in updates from upstream, but for now it's good to know I can... and have my own familiar PR-based workflow against my own org's repo.

# time to start WRITING IN THE BLOG! :)

# 20200508

## devnotes -> blog post(s) thoughts
  √ start to org the notes above into an outline for post about the issues w setup here.
  maybe somewhat evergreen (not just "this version of this lib, howto") bc touches on core toolset
  be sure to maintain polite, humble "this worked for me" and avoid being strident or accusatory
  not looking to debate merits of npm vs yarn, nor the rest, just aiming to help, and be a voice of reason.
  moving fwd, want to learn to capture such notes as blog fodder efficiently.
  devnotes while doing stuff, mos def.
  zsh history has all the cmds, and git commits are useful places to add context and batch stuff
  right now it's 756a -- way over the allotted time, and this wasn't ideal -- but the win is that I wanted to work on blog-related stuff, and I did.
  now to catch up w my day!

  speaking of capturing knowledge as I go -- Jan ski trip eslint+prettier stuff and "nextapp" for TL, took long time to dig it up.
  ~/work/torchlight/ui-arch/devnotes.md
  ~/work/torchlight/ui-arch/nextapp
  phew!
  TODO - revisit that stuff and incorp properly into memex etc.
  and turn the "eslint+prettier" stuff into blog post too.
  /829a

  ... and beyond the eslint+prettier howto, those devnotes have a solution for the peerdeps issue too, ie the curr WIP blog post abt npm globals etc! :)

  833 really time to pause now. but wow will it feel good to pull together my notes and kb etc, empowered and expert, I've got this.
  and it will be FUN to build stuff w great tools I grok and config for FLOWSTATE.
  and to help other ppl suffer less bc I went thru it and saw it and can express it! :)
  835

  lots of helpful kb stuff in my ChrisWeekly.com private trello from the TL project, eg
  https://trello.com/c/N7rmjNYS/29-ssr-css-classnames-labels
  etc
  /846a

## WIP - DEV AND DEPLOY

√ tweak scripts - eg `yarn dev`
√ tweak /etc/hosts - 127.0.0.1 dev.local -> http://dev.local:8000
. 


