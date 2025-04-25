---
layout: post
title: "git bisect"
description: ""
tags: ['git','git-bisect']
---

<p>
<h2>Git Bisect</h2>
<a href="https://git-scm.com/docs/git-bisect">Docs</a>

  After using git bisect to find when something was removed I truely see the power behind it. It is simple straight forward and will save you time and energy by using simple algorithms you should fully understand already (a binary search tree).

  To begin: `git bisect start` will start a git-bisect "session", assume the tree you are working on is more or less read-only and do not look to change it but use it to debug.

  When debugging you usually know 1 thing pretty well, what is wrong. Which is very commonly the current/latest commit.

  So our next step is to tell git bisect just that: `git bisect bad` (which assumes the current commit at the end of this command) otherwise you can actually provide it a specific commit after.

  Next is a bit harder, when was it last working?

  It might be a good time to split off a worktree and double check when the last time you _confidently_ know it was working as intended.

  After establishing when it was last working you then tell git: `git bisect good <commit or tag>`.

  Now git knows where to begin, and in typical binary search you start at the middle and cut everything down by 50% each time.

  After entering that command git will have changed the current checkout to the middle commit between the good and the bad commit.

  Now it is your turn, is this a good commit or a bad commit? And then simply tell it `git bisect <good or bad>`.

  It will then continue to give you a few more commits (few is relative to how many commits were between your good and bad points).

  At the end it will hand you the commit that caused the issue. `2f73d5e801821c3f9d7b62afc86c78d455b24caf is the first bad commit`

  Git still amazes me with how intuitive & simple some of it's more complicated commands can be while dealing with such complex problems.
</p>
