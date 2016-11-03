---
layout: post
title: "Fixing Corrupted git blobs"
description: ""
tags: []
---

<p>Git is an amazing tool.
It single handedly helps bring everyone better software by aiding developers in
keeping track of changes to their source code, and allow others to make change
suggestions. This being said like every software you can find a way to break it.
I managed to find a way, of course lol. The biggest thing I am going to stress
before continuing is the point behind keeping your git repo synced as often as
possible and with multiple locations (&gt;1). When using git and an autimated
tool like syncthing or bittorrent sync to keep your repository synced between
multiple computers git+sync can make mistakes (mostly non-information). What I
mean by that is the user can push/pull at a time when sync is not finished,
syncing, and enevitably can corrupt your repositories. As I said keeping your
repository in multiple places really helps!</p><p>When I came across this the
hard way I remembered how I used to sync my repositories between the computer
and a usb I carry around with <code>rsync</code>. This tool is beautiful also,
though I think most software is, it finds the differences between two folders
and adds the required files. If a file differs it overwrites the file (this can
be dangerous if you want both versions). But the way I am going to show you how
to fix git both files should be identical, and therefore rsync is perfect. If
you manage to corrupt the <i>bare</i> repository on the usb but the repository
on the coputer is fine this is for you.
<code>rsync -avz <from(good repo)> <to(bad repo)></code>
is the syntax for <code>rsync</code>.
The <code>-a</code> flag literally means archive.
The <code>-v</code> is standard verbose.
The <code>-z</code> is compress which can help with speed.</p>
<h2>Please read this it's imporant</h2>
A git repo which you can see your files is a normal git repository which git
stores it's bare repository in the .git directory (so you don't have to see git
do its magic). A bare repo is essentially just the .git without any of your
files showing, there are more differences but that is what you need to know to
continue.
<p>Now for the fix:
<h4>Normal(good) to Bare(bad)</h4>
<code>rsync -avz /path/to/repo/.git/objects /path/to/bad/repo/objects</code>
<h4>Normal(good) to Normal(bad)</h4>
<code>rsync -avz /path/to/repo/.git/objects /path/to/bad/repo/.git/objects</code>
<h4>Bare(good) to Normal(bad)</h4>
<code>rsync -avz /path/to/repo/objects /path/to/bad/repo/.git/objects</code>
<h4>Bare(good) to Bare(bad)</h4>
<code>rsync -avz /path/to/repo/objects /path/to/bad/repo/objects</code>.</p>
<p>
To understand what I did you have to understand a little more of how git works.
When you make changes git stores your files within a file, the file name is the
hash of your file. Those hashes are stored under the objects directory within a
git repository, and are usually called blobs. If you look into the objects
directory you will see 244 directories which have names starting with
00, 01, 02 and running until ff. This is the start of the file's hash when you
dive into one of these directories you might (depending on your luck & size of
the repo) will see files named with the rest of the hash. Thusly, 00(directory)
+ 18d2628a050251b51fa7dbbc125b667a466a43(file) is the full hash. If you know
hashes the likely hood of a collition is very small though with the nature of
hashes it's unavoidable. Hashes take a file or string and in essence add up the
bits and give you back a string that 99.9999% uniquely identifies it from any
other string/file. Git uses this to do it's magic.
Allowing a user to modify & share a file and "never" have that modification
collide with any other user's modifications even to the same file.
