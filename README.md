# stackexchange

A bash script for converting stackexchange forums to json.

## Requirements

 - [hgrep](https://github.com/TUVIMEN/hgrep)
 - [jq](https://github.com/stedolan/jq)

## Installation
    
    install -m 755 stackexchange /usr/bin

## Supported sites

All supported sites can be found [here](https://stackexchange.com/sites)

## Supported links formats

    https://example.stackexchange.com
    https://stackoverflow.com
    https://example.stackexchange.com/questions
    https://example.stackexchange.com/tags
    https://example.stackexchange.com/questions/tagged/some-tag
    https://example.stackexchange.com/questions/19924
    https://example.stackexchange.com/questions/19924/issue-title

## Json format

Here's json from one of the most popular issues on [stackoverflow](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git):
    
```
{
  "link": "https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git",
  "title": "How do I undo the most recent local commits in Git?",
  "views": "11914565",
  "bookmarks": "",
  "tags": [
    "git",
    "version-control",
    "git-commit",
    "undo"
  ],
  "posts": [
    {
      "rating": "25017",
      "date": "",
      "edited": "2022-10-12 10:47:00Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "                <p>I accidentally committed the wrong files to <a href=\"https://en.wikipedia.org/wiki/Git\" rel=\"noreferrer\">Git</a>, but didn't push the commit to the server yet.</p><p>How do I undo those commits from the <em>local</em> repository?</p>    ",
      "comments": [
        {
          "content": "You know what git needs? <code>git undo</code>, that&#39;s it. Then the reputation git has for handling mistakes made by us mere mortals disappears. Implement by pushing the current state on a git stack before executing any <code>git</code> command. It would affect performance, so it would be best to add a config flag as to whether to enable it.",
          "date": "2018-03-20 01:45:28Z",
          "author": "Yimin Rong",
          "reputation": "1778"
        },
        {
          "content": "@YiminRong That can be done with Git&#39;s <code>alias</code> feature: <a href=\"https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases\" rel=\"nofollow noreferrer\">git-scm.com/book/en/v2/Git-Basics-Git-Aliases</a>",
          "date": "2018-10-05 14:50:08Z",
          "author": "Edric",
          "reputation": "22742"
        },
        {
          "content": "For VsCode users , just type ctrl +shift +G and then click on three dot ,ie , more options and then click on undo Last Commit",
          "date": "2019-04-08 12:15:40Z",
          "author": "ashad",
          "reputation": "351"
        },
        {
          "content": "@YiminRong Undo <i>what</i> exactly? There are dozens of very different functional cases where &quot;undoing&quot; means something <b>completely</b> different. I&#39;d bet adding a new fancy &quot;magic wand&quot; would only confuse things more.",
          "date": "2020-03-24 14:27:03Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        },
        {
          "content": "@YiminRong Not buying it. People would still fumble and undo things not to be undone. But more importantly, <code>git reflog</code> is already close to what you describe, but gives the user more control on what&#39;s to be (un)done. But please, no, &quot;undo&quot; does not work the same everywhere, and people would <i>expect</i> many different things for the feature to achieve. Undo last commit? Undo last action? If last action was a push, undo how exactly, (reset and push) or (revert and push)?",
          "date": "2020-03-25 13:23:33Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        }
      ]
    },
    {
      "rating": "27270",
      "date": "",
      "edited": "2022-04-13 15:03:38Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h1>Undo a commit &amp; redo</h1><pre class=\"lang-sh prettyprint-override\"><code>$ git commit -m &quot;Something terribly misguided&quot; # (0: Your Accident)$ git reset HEAD~                              # (1)[ edit files as necessary ]                    # (2)$ git add .                                    # (3)$ git commit -c ORIG_HEAD                      # (4)</code></pre><ol><li><a href=\"https://git-scm.com/docs/git-reset\" rel=\"noreferrer\"><code>git reset</code></a> is the command responsible for the <strong>undo</strong>. It will undo your last commit while <strong>leaving your working tree (the state of your files on disk) untouched.</strong> You'll need to add them again before you can commit them again).</li><li>Make corrections to <a href=\"https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified#_the_working_directory\" rel=\"noreferrer\">working tree</a> files.</li><li><a href=\"https://git-scm.com/docs/git-add\" rel=\"noreferrer\"><code>git add</code></a> anything that you want to include in your new commit.</li><li>Commit the changes, reusing the old commit message. <code>reset</code> copied the old head to <code>.git/ORIG_HEAD</code>; <a href=\"https://git-scm.com/docs/git-commit#Documentation/git-commit.txt--cltcommitgt\" rel=\"noreferrer\"><code>commit</code> with <code>-c ORIG_HEAD</code></a> will open an editor, which initially contains the log message from the old commit and allows you to edit it. If you do not need to edit the message, you could use the <a href=\"https://git-scm.com/docs/git-commit#Documentation/git-commit.txt--Cltcommitgt\" rel=\"noreferrer\"><code>-C</code></a> option.</li></ol><p><strong>Alternatively, to edit the previous commit (or just its commit message)</strong>, <a href=\"https://stackoverflow.com/q/179123/1146608\"><code>commit --amend</code></a> will add changes within the current index to the previous commit.</p><p><strong>To remove (not revert) a commit that has been pushed to the server</strong>, rewriting history with <code>git push origin main --force[-with-lease]</code> is necessary. <a href=\"https://stackoverflow.com/q/52823692/4096791\">It's <em>almost always</em> a bad idea to use <code>--force</code>; prefer <code>--force-with-lease</code></a> instead, and as noted in <a href=\"https://git-scm.com/docs\" rel=\"noreferrer\">the git manual</a>:</p><blockquote><p>You should understand the implications of rewriting history if you [rewrite history] has already been published.</p></blockquote><hr /><h2>Further Reading</h2><p><a href=\"https://stackoverflow.com/questions/34519665/how-to-move-head-back-to-a-previous-location-detached-head/34519716#34519716\">You can use <code>git reflog</code> to determine the SHA-1</a> for the commit to which you wish to revert. Once you have this value, use the sequence of commands as explained above.</p><hr /><p><code>HEAD~</code> is the same as <code>HEAD~1</code>. The article <a href=\"https://stackoverflow.com/a/46350644/5175709\">What is the HEAD in git?</a> is helpful if you want to uncommit multiple commits.</p>    ",
      "comments": [
        {
          "content": "And if the commit was to the wrong branch, you may <code>git checkout theRightBranch</code> with all the changes stages. As I just had to do.",
          "date": "2010-10-05 15:44:20Z",
          "author": "Frank Shearar",
          "reputation": "16897"
        },
        {
          "content": "If you&#39;re working in DOS, instead of <code>git reset --soft HEAD^</code> you&#39;ll need to use <code>git reset --soft HEAD~1</code>.  The ^ is a continuation character in DOS so it won&#39;t work properly.  Also, <code>--soft</code> is the default, so you can omit it if you like and just say <code>git reset HEAD~1</code>.",
          "date": "2011-04-13 14:15:10Z",
          "author": "Ryan Lundy",
          "reputation": "199856"
        },
        {
          "content": "zsh users might get: <code>zsh: no matches found: HEAD^</code> - you need to escape ^ i.e. <code>git reset --soft HEAD\\^</code>",
          "date": "2013-02-21 17:47:56Z",
          "author": "tnajdek",
          "reputation": "542"
        },
        {
          "content": "The answer is not correct if, say by accident, <code>git commit -a</code> was issued when the <code>-a</code> should have been left out.  In which case, it&#39;s better no leave out the <code>--soft</code> (which will result in <code>--mixed</code> which is the default) and then you can restage the changes you meant to commit.",
          "date": "2014-07-02 21:19:30Z",
          "author": "dmansfield",
          "reputation": "1068"
        },
        {
          "content": "I&#39;ve googled &amp; hit this page about 50 times, and I always chuckle at the first line of code <code>git commit -m &quot;Something terribly misguided&quot;</code>",
          "date": "2019-08-21 04:25:55Z",
          "author": "100pic",
          "reputation": "387"
        }
      ]
    },
    {
      "rating": "12194",
      "date": "2011-07-28 22:22:20Z",
      "edited": "2022-10-12 10:50:29Z",
      "author": {
        "name": "Ryan Lundy",
        "reputation": "200k",
        "gbadge": "37",
        "sbadge": "181",
        "bbadge": "209"
      },
      "editor": {
        "name": "Nitin",
        "reputation": "2,487",
        "gbadge": "",
        "sbadge": "25",
        "bbadge": "56"
      },
      "content": "<p>Undoing a commit is a little scary if you don't know how it works.  But it's actually amazingly easy if you do understand. I'll show you the 4 different ways you can undo a commit.</p><p>Say you have this, where C is your HEAD and (F) is the state of your files.</p><pre><code>   (F)A-B-C    ↑  master</code></pre><h3>Option 1: <code>git reset --hard</code></h3><p>You want to <strong>destroy commit C and also throw away any uncommitted changes</strong>.  You do this:</p><pre><code>git reset --hard HEAD~1</code></pre><p>The result is:</p><pre><code> (F)A-B  ↑master</code></pre><p>Now B is the HEAD.  Because you used <code>--hard</code>, your files are reset to their state at commit B.</p><h3>Option 2: <code>git reset</code></h3><p>Maybe commit C wasn't a disaster, but just a bit off.  You want to <strong>undo the commit but keep your changes</strong> for a bit of editing before you do a better commit.  Starting again from here, with C as your HEAD:</p><pre><code>   (F)A-B-C    ↑  master</code></pre><p>Do this, leaving off the <code>--hard</code>:</p><pre><code>git reset HEAD~1</code></pre><p>In this case, the result is:</p><pre><code>   (F)A-B-C  ↑master</code></pre><p>In both cases, HEAD is just a pointer to the latest commit.  When you do a <code>git reset HEAD~1</code>, you tell Git to move the HEAD pointer back one commit.  But (unless you use <code>--hard</code>) you leave your files as they were.  So now <code>git status</code> shows the changes you had checked into C.  You haven't lost a thing!</p><h3>Option 3: <code>git reset --soft</code></h3><p>For the lightest touch, you can even <strong>undo your commit but leave your files and your <a href=\"https://git.wiki.kernel.org/index.php/WhatIsTheIndex\" rel=\"noreferrer\">index</a></strong>:</p><pre><code>git reset --soft HEAD~1</code></pre><p>This not only leaves your files alone, it even leaves your <em>index</em> alone.  When you do <code>git status</code>, you'll see that the same files are in the index as before.  In fact, right after this command, you could do <code>git commit</code> and you'd be redoing the same commit you just had.</p><h3>Option 4: you did <code>git reset --hard</code> and need to get that code back</h3><p>One more thing: Suppose you <strong>destroy a commit</strong> as in the first example, <strong>but then discover you needed it after all</strong>?  Tough luck, right?</p><p>Nope, there's <em>still</em> a way to get it back.  Type this</p><pre><code>git reflog</code></pre><p>and you'll see a list of (partial) commit <a href=\"https://en.wikipedia.org/wiki/SHA-1#Data_integrity\" rel=\"noreferrer\">shas</a> (that is, hashes) that you've moved around in.  Find the commit you destroyed, and do this:</p><pre><code>git checkout -b someNewBranchName shaYouDestroyed</code></pre><p>You've now resurrected that commit.  Commits don't actually get destroyed in Git for some 90 days, so you can usually go back and rescue one you didn't mean to get rid of.</p>    ",
      "comments": [
        {
          "content": "BEWARE! This might not do what you expect if your erroneous commit was a (fast-forward) merge! If your head is on a merge commit (ex: merged branch feature into master), <code>git reset --hard~1</code> will point the master branch to the last commit inside the feature branch. In this case the specific commit ID should be used instead of the relative command.",
          "date": "2013-02-20 18:46:35Z",
          "author": "Chris Kerekes",
          "reputation": "1118"
        },
        {
          "content": "Consider noting that the number in <code>HEAD~1</code> can be substituted to any positive integer, e.g. <code>HEAD~3</code>. It may seem obvious, but beginners (like me) are very careful when running git commands, so they may not want to risk messing something up by testing this stuff themselves.",
          "date": "2013-08-13 14:37:26Z",
          "author": "&#x160;ime Vidas",
          "reputation": "177346"
        },
        {
          "content": "Missing a crucial point: If the said commit was previously &#39;pushed&#39; to the remote, any &#39;undo&#39; operation, no matter how simple, will cause enormous pain and suffering to the rest of the users who have this commit in their local copy, when they do a &#39;git pull&#39; in the future.  So, if the commit was already &#39;pushed&#39;, do this instead:      git revert &lt;bad-commit-sha1-id&gt;     git push origin :",
          "date": "2013-11-08 23:43:18Z",
          "author": "FractalSpace",
          "reputation": "5313"
        },
        {
          "content": "@FractalSpace, it won&#39;t cause &quot;enormous pain and suffering.&quot;  I&#39;ve done a few force pushes when using Git with a team.  All it takes is communication.",
          "date": "2013-11-09 00:00:10Z",
          "author": "Ryan Lundy",
          "reputation": "199856"
        },
        {
          "content": "@Kyralessa In my workplace, messing up entire team&#39;s workflow and then telling them how to fix sh*t is not called &#39;communication&#39;. git history re-write is a destructive operation that results in trashing of parts of the repo. Insisting on its use, while clear and safe alternatives are available is simply irresponsible.",
          "date": "2013-11-09 03:02:51Z",
          "author": "FractalSpace",
          "reputation": "5313"
        }
      ]
    },
    {
      "rating": "2575",
      "date": "2011-06-16 17:27:23Z",
      "edited": "2020-06-20 09:12:55Z",
      "author": {
        "name": "Andrew",
        "reputation": "219k",
        "gbadge": "190",
        "sbadge": "506",
        "bbadge": "694"
      },
      "editor": {
        "name": "Community",
        "reputation": "1",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>There are two ways to &quot;undo&quot; your last commit, depending on whether or not you have already made your commit public (pushed to your remote repository):</p><h2>How to undo a local commit</h2><p>Let's say I committed locally, but now I want to remove that commit.</p><pre><code>git log    commit 101: bad commit    # Latest commit. This would be called 'HEAD'.    commit 100: good commit   # Second to last commit. This is the one we want.</code></pre><p>To restore everything back to the way it was prior to the last commit, we need to <code>reset</code> to the commit before <code>HEAD</code>:</p><pre><code>git reset --soft HEAD^     # Use --soft if you want to keep your changesgit reset --hard HEAD^     # Use --hard if you don't care about keeping the changes you made</code></pre><p>Now <code>git log</code> will show that our last commit has been removed.</p><h2>How to undo a public commit</h2><p>If you have already made your commits public, you will want to create a new commit which will &quot;revert&quot; the changes you made in your previous commit (current HEAD).</p><pre><code>git revert HEAD</code></pre><p>Your changes will now be reverted and ready for you to commit:</p><pre><code>git commit -m 'restoring the file I removed by accident'git log    commit 102: restoring the file I removed by accident    commit 101: removing a file we don't need    commit 100: adding a file that we need</code></pre><p>For more information, check out <em><a href=\"https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things\" rel=\"noreferrer\">Git Basics - Undoing Things</a></em>.</p>    ",
      "comments": [
        {
          "content": "I found this answer the clearest. <code>git revert HEAD^</code> is not the previous, is the previous of the previous. I did : <code>git revert HEAD</code> and then push again and it worked :)",
          "date": "2011-07-14 08:32:53Z",
          "author": "nacho4d",
          "reputation": "42363"
        },
        {
          "content": "If Git asks you &quot;More?&quot; when you try these commands, use the alternate syntax on this answer: <a href=\"https://stackoverflow.com/a/14204318/823470\">stackoverflow.com/a/14204318/823470</a>",
          "date": "2020-03-05 15:50:09Z",
          "author": "tar",
          "reputation": "1509"
        },
        {
          "content": "<code>revert</code> deleted some files I add added to my repo. Use it with caution!",
          "date": "2021-07-31 22:25:00Z",
          "author": "carloswm85",
          "reputation": "944"
        },
        {
          "content": "whats the difference between git reset --soft HEAD~1 and reset --soft HEAD^?",
          "date": "2021-12-21 20:23:41Z",
          "author": "Jorge Tovar",
          "reputation": "788"
        },
        {
          "content": "Maybe I just didn&#39;t see it, but the last <a href=\"https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things\" rel=\"nofollow noreferrer\">link</a> you provided doesn&#39;t seem to cover this specific question. (I know you said it was additional information, I&#39;m just wondering whether I missed something.)",
          "date": "2022-01-13 09:01:53Z",
          "author": "NerdOnTour",
          "reputation": "486"
        }
      ]
    },
    {
      "rating": "1896",
      "date": "2009-05-29 18:16:26Z",
      "edited": "2016-10-12 06:45:55Z",
      "author": {
        "name": "bdonlan",
        "reputation": "219k",
        "gbadge": "29",
        "sbadge": "264",
        "bbadge": "321"
      },
      "editor": {
        "name": "Vikrant",
        "reputation": "5,360",
        "gbadge": "17",
        "sbadge": "48",
        "bbadge": "72"
      },
      "content": "<p>Add/remove files to get things the way you want:</p><pre><code>git rm classdirgit add sourcedir</code></pre><p>Then amend the commit:</p><pre><code>git commit --amend</code></pre><p>The previous, erroneous commit will be edited to reflect the new index state - in other words, it'll be like you never made the mistake in the first place.</p><p>Note that you should only do this if you haven't pushed yet. If you have pushed, then you'll just have to commit a fix normally.</p>    ",
      "comments": [
        {
          "content": "FYI: This removes all my files and I lost changes.",
          "date": "2020-05-15 10:03:11Z",
          "author": "egorlitvinenko",
          "reputation": "2706"
        },
        {
          "content": "UPD: However, I&#39;ve restored it using reflog. But the receipt did not work for the initial commit.",
          "date": "2020-05-15 11:25:56Z",
          "author": "egorlitvinenko",
          "reputation": "2706"
        },
        {
          "content": "Use <code>git rm --cached</code> to keep the files in the filesystem and only delete them from the git index!",
          "date": "2020-05-19 10:00:55Z",
          "author": "xuiqzy",
          "reputation": "167"
        },
        {
          "content": "&quot; git --amend --no-edit &quot; will commit your last changes to the commit before them.",
          "date": "2022-03-26 22:22:20Z",
          "author": "alex",
          "reputation": "499"
        }
      ]
    },
    {
      "rating": "1227",
      "date": "2009-05-29 18:13:07Z",
      "edited": "2022-09-09 08:13:22Z",
      "author": {
        "name": "Lennart Koopmann",
        "reputation": "19.7k",
        "gbadge": "4",
        "sbadge": "25",
        "bbadge": "33"
      },
      "editor": {
        "name": "ChrisW",
        "reputation": "54.1k",
        "gbadge": "12",
        "sbadge": "114",
        "bbadge": "215"
      },
      "content": "<p>This will add a new commit which deletes the added files.</p><pre class=\"lang-bash prettyprint-override\"><code>git rm yourfiles/*.classgit commit -a -m &quot;deleted all class files in folder 'yourfiles'&quot;</code></pre><hr /><p>Or you can rewrite history to undo the last commit.</p><p><strong>Warning</strong>: this command will <strong>permanently remove</strong> the modifications to the <code>.java</code> files (and any other files) that you committed -- and delete all your changes from your working directory:</p><pre><code>git reset --hard HEAD~1</code></pre><p>The <code>hard reset</code> to <code>HEAD-1</code> will set your working copy to the state of the commit before your wrong commit.</p>    ",
      "comments": [
        {
          "content": "<code>git commit -a -m &quot;&quot;</code> or <code>git commit -am &quot;&quot;</code> naturally! :]",
          "date": "2014-06-21 16:31:59Z",
          "author": "trejder",
          "reputation": "16770"
        },
        {
          "content": "Another &#39;shortcut&#39; use of stash; if you want to unstage everything (undo git add), just <code>git stash</code>, then <code>git stash pop</code>",
          "date": "2015-12-08 22:30:29Z",
          "author": "seanriordan08",
          "reputation": "296"
        },
        {
          "content": "Gosh... this answer should be deleted. <code>git reset --hard HEAD-1</code> is incredibly dangerous - it&#39;s not just &quot;undoing the commit&quot;, it also delete all of your changes, which is not what OP asked for. I unfortunately applied this answer (which StackOverflow for no reason shows first than the accepted one with 26k upvotes), and now will struggle to recover all of my changes.",
          "date": "2022-06-07 13:19:25Z",
          "author": "Jack",
          "reputation": "1439"
        },
        {
          "content": "For those who did not read and just ran the above command like me, I hope you learned your lesson now XD... To undo command, <code>git reflog</code> to find the discarded commit and run <code>git reset --hard $1</code> where <code>$1</code> is your discarded commit",
          "date": "2022-08-11 04:10:50Z",
          "author": "seantsang",
          "reputation": "935"
        }
      ]
    },
    {
      "rating": "915",
      "date": "2010-07-31 09:39:13Z",
      "edited": "2021-07-23 20:52:02Z",
      "author": {
        "name": "Zaz",
        "reputation": "44.7k",
        "gbadge": "11",
        "sbadge": "80",
        "bbadge": "97"
      },
      "editor": {
        "name": "Zoe stands with Ukraine",
        "reputation": "25.9k",
        "gbadge": "20",
        "sbadge": "115",
        "bbadge": "148"
      },
      "content": "<h2 id=\"to-change-the-last-commit-ayg9\">To change the last commit</h2><p>Replace the files in the index:</p><pre><code>git rm --cached *.classgit add *.java</code></pre><p>Then, if it's a private branch, <strong>amend</strong> the commit:</p><pre><code>git commit --amend</code></pre><p>Or, if it's a shared branch, make a new commit:</p><pre><code>git commit -m 'Replace .class files with .java files'</code></pre><br/><p><em>(<strong>To change a previous commit</strong>, use the awesome <a href=\"https://stackoverflow.com/a/28421811/405550\">interactive rebase</a>.)</em></p><hr /><p>ProTip™: Add <code>*.class</code> to a <a href=\"https://help.github.com/articles/ignoring-files\" rel=\"noreferrer\">gitignore</a> to stop this happening again.</p><hr /><h2 id=\"to-revert-a-commit-rotb\">To revert a commit</h2><p>Amending a commit is the ideal solution if you need to change the last commit, but a more general solution is <code>reset</code>.</p><p>You can reset Git to any commit with:</p><pre><code>git reset @~N</code></pre><p>Where <code>N</code> is the number of commits before <code>HEAD</code>, and <code>@~</code> resets to the previous commit.</p><p>Instead of amending the commit, you could use:</p><pre><code>git reset @~git add *.javagit commit -m &quot;Add .java files&quot;</code></pre><p>Check out <code>git help reset</code>, specifically the sections on <code>--soft</code> <code>--mixed</code> and <code>--hard</code>, for a better understanding of what this does.</p><h2 id=\"reflog-hdrm\">Reflog</h2><p>If you mess up, you can always use the reflog to find dropped commits:</p><pre><code>$ git reset @~$ git reflogc4f708b HEAD@{0}: reset: moving to @~2c52489 HEAD@{1}: commit: added some .class files$ git reset 2c52489... and you're back where you started</code></pre><br/>    ",
      "comments": [
        {
          "content": "For those reading in future - please note that <code>git revert</code> is a separate command - which basically &#39;resets&#39; a single commimt.",
          "date": "2018-08-08 07:11:00Z",
          "author": "BenKoshy",
          "reputation": "31287"
        }
      ]
    },
    {
      "rating": "804",
      "date": "2012-05-25 16:04:29Z",
      "edited": "2019-11-21 13:47:43Z",
      "author": {
        "name": "Jaco Pretorius",
        "reputation": "25k",
        "gbadge": "11",
        "sbadge": "60",
        "bbadge": "93"
      },
      "editor": {
        "name": "Peter Mortensen",
        "reputation": "30.6k",
        "gbadge": "21",
        "sbadge": "102",
        "bbadge": "125"
      },
      "content": "<p>Use <code>git revert &lt;commit-id&gt;</code>.</p><p>To get the commit ID, just use <code>git log</code>.</p>    ",
      "comments": [
        {
          "content": "What does that mean, cherry pick the commit? In my case, I was on the wrong branch when I edited a file. I committed it then realized I was in the wrong branch. Using &quot;git reset --soft HEAD~1&quot; got me back to just before the commit, but now if I checkout the correct branch, how do I undo the changes to the file in wrong branch but instead make them (in the same named file) in the correct branch?",
          "date": "2015-01-13 22:05:57Z",
          "author": "astronomerdave",
          "reputation": "510"
        },
        {
          "content": "I just utilized <code>git revert commit-id</code> worked like a charm.  Of course then you will need to push your changes.",
          "date": "2016-01-25 21:07:16Z",
          "author": "Casey Robinson",
          "reputation": "3259"
        },
        {
          "content": "I believe that would be <code>git cherry-pick &lt;&lt;erroneous-commit-sha&gt;&gt;</code> @astronomerdave. From, Mr. Almost-2-Years-Late-to-the-Party.",
          "date": "2016-10-20 18:19:50Z",
          "author": "Tom Howard",
          "reputation": "4382"
        },
        {
          "content": "@Kris: Instead of cherry-pick use rebase. Because it is advanced cherry-picking",
          "date": "2018-11-10 09:38:58Z",
          "author": "Eugen Konkov",
          "reputation": "20020"
        },
        {
          "content": "I&#39;d use revert only if I&#39;ve already pushed my commit. Otherwise, reset is a better option. Don&#39;t forget that revert creates a new commit, and usually this is not the goal.",
          "date": "2020-02-06 12:40:43Z",
          "author": "Hola Soy Edu Feliz Navidad",
          "reputation": "6440"
        }
      ]
    },
    {
      "rating": "660",
      "date": "2013-01-31 07:06:10Z",
      "edited": "2020-11-21 14:11:02Z",
      "author": {
        "name": "Madhan Ayyasamy",
        "reputation": "15k",
        "gbadge": "3",
        "sbadge": "17",
        "bbadge": "18"
      },
      "editor": {
        "name": "riQQ",
        "reputation": "7,668",
        "gbadge": "5",
        "sbadge": "37",
        "bbadge": "55"
      },
      "content": "<p>If you are planning to undo a local commit entirely, whatever you change you did on the commit, and if you don't worry anything about that, just do the following command.</p><pre><code>git reset --hard HEAD^1</code></pre><p>(This command will ignore your entire commit and your changes will be lost completely from your local working tree). If you want to undo your commit, but you want your changes in the staging area (before commit just like after <code>git add</code>) then do the following command.</p><pre><code>git reset --soft HEAD^1</code></pre><p>Now your committed files come into the staging area. Suppose if you want to upstage the files, because you need to edit some wrong content, then do the following command</p><pre><code>git reset HEAD</code></pre><p>Now committed files to come from the staged area into the unstaged area. Now files are ready to edit, so whatever you change, you want to go edit and added it and make a fresh/new commit.</p><p><a href=\"http://madhan-tech-updates.blogspot.in/2013/01/how-to-undo-your-local-git-commit.html\" rel=\"noreferrer\">More (link broken)</a> (<a href=\"https://web.archive.org/web/20170410191943/http://madhan-tech-updates.blogspot.in/2013/01/how-to-undo-your-local-git-commit.html\" rel=\"noreferrer\">Archived version</a>)</p>    ",
      "comments": [
        {
          "content": "@SMR, In your example, all are pointing into current HEAD only. HEAD^ = HEAD^1. As well as HEAD^1 = HEAD~1.  When you use HEAD~2, there is a difference between ~ and ^ symbols. If you use ~2 means “the first parent of the first parent,” or “the grandparent”.",
          "date": "2015-12-14 15:34:11Z",
          "author": "Madhan Ayyasamy",
          "reputation": "15010"
        },
        {
          "content": "git reset --hard HEAD^1 gives me this error &quot;fatal: ambiguous argument &#39;HEAD1&#39;: unknown revision or path not in the working tree.&quot;",
          "date": "2020-12-20 10:55:17Z",
          "author": "Rob Mosher",
          "reputation": "370"
        }
      ]
    },
    {
      "rating": "595",
      "date": "2011-12-13 10:18:31Z",
      "edited": "2019-11-21 13:46:26Z",
      "author": {
        "name": "nickf",
        "reputation": "527k",
        "gbadge": "198",
        "sbadge": "643",
        "bbadge": "720"
      },
      "editor": {
        "name": "Peter Mortensen",
        "reputation": "30.6k",
        "gbadge": "21",
        "sbadge": "102",
        "bbadge": "125"
      },
      "content": "<p>If you have <a href=\"https://github.com/visionmedia/git-extras\" rel=\"noreferrer\">Git Extras</a> installed, you can run <code>git undo</code> to undo the latest commit. <code>git undo 3</code> will undo the last three commits.</p>    ",
      "comments": []
    },
    {
      "rating": "557",
      "date": "2012-04-06 13:58:52Z",
      "edited": "2019-11-21 13:47:00Z",
      "author": {
        "name": "neoneye",
        "reputation": "48.7k",
        "gbadge": "24",
        "sbadge": "161",
        "bbadge": "151"
      },
      "editor": {
        "name": "Peter Mortensen",
        "reputation": "30.6k",
        "gbadge": "21",
        "sbadge": "102",
        "bbadge": "125"
      },
      "content": "<p>I wanted to undo the latest five commits in our shared repository. I looked up the revision id that I wanted to rollback to. Then I typed in the following.</p><pre><code>prompt&gt; git reset --hard 5a7404742c85HEAD is now at 5a74047 Added one more page to catalogueprompt&gt; git push origin master --forceTotal 0 (delta 0), reused 0 (delta 0)remote: bb/acl: neoneye is allowed. accepted payload.To git@bitbucket.org:thecompany/prometheus.git + 09a6480...5a74047 master -&gt; master (forced update)prompt&gt;</code></pre>    ",
      "comments": [
        {
          "content": "Rewriting history on a shared repository is generally a very bad idea.  I assume you know what you&#39;re doing, I just hope future readers do too.",
          "date": "2012-12-07 16:02:12Z",
          "author": "Brad Koch",
          "reputation": "18438"
        },
        {
          "content": "Yes rollback is dangerous. Make sure that your working copy is in the desired state before you push. When pushing then the unwanted commits gets deleted permanently.",
          "date": "2012-12-08 14:14:43Z",
          "author": "neoneye",
          "reputation": "48667"
        },
        {
          "content": "&quot;Just like in the real world, if you want to rewrite history, you need a conspiracy: everybody has to be &#39;in&#39; on the conspiracy (at least everybody who knows about the history, i.e. everybody who has ever pulled from the branch).&quot; Source: <a href=\"http://stackoverflow.com/a/2046748/334451\">stackoverflow.com/a/2046748/334451</a>",
          "date": "2013-08-07 10:10:25Z",
          "author": "Mikko Rantalainen",
          "reputation": "12561"
        }
      ]
    },
    {
      "rating": "528",
      "date": "2012-10-25 03:41:20Z",
      "edited": "2018-12-06 21:59:20Z",
      "author": {
        "name": "Zombo",
        "reputation": "1",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "Peter Mortensen",
        "reputation": "30.6k",
        "gbadge": "21",
        "sbadge": "102",
        "bbadge": "125"
      },
      "content": "<p>I prefer to use <code>git rebase -i</code> for this job, because a nice list pops up where I can choose the commits to get rid of. It might not be as direct as some other answers here, but it just <em>feels right</em>.</p><p>Choose how many commits you want to list, then invoke like this (to enlist last three)</p><pre><code>git rebase -i HEAD~3</code></pre><p>Sample list</p><pre><code>pick aa28ba7 Sanity check for RtmpSrv portpick c26c541 RtmpSrv version optionpick 58d6909 Better URL decoding support</code></pre><p>Then Git will remove commits for any line that you remove.</p>    ",
      "comments": []
    },
    {
      "rating": "496",
      "date": "",
      "edited": "2018-12-07 05:57:27Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h2>How to fix the previous local commit</h2><p>Use git-gui (or similar) to perform a <code>git commit --amend</code>. From the GUI you can add or remove individual files from the commit. You can also modify the commit message. </p><h2>How to undo the previous local commit</h2><p>Just reset your branch to the previous location (for example, using <code>gitk</code> or <code>git rebase</code>). Then reapply your changes from a saved copy. After garbage collection in your local repository, it will be like the unwanted commit never happened. To do all of that in a single command, use <code>git reset HEAD~1</code>.</p><p><strong>Word of warning</strong>: <em>Careless use of <code>git reset</code> is a good way to get your working copy into a confusing state. I recommend that Git novices avoid this if they can.</em></p><h2>How to undo a public commit</h2><p>Perform a <a href=\"https://stackoverflow.com/a/1624724/86967\">reverse cherry pick</a> (<a href=\"http://git-scm.com/docs/git-revert.html\" rel=\"noreferrer\">git-revert</a>) to undo the changes.</p><p>If you haven't yet pulled other changes onto your branch, you can simply do...</p><pre><code>git revert --no-edit HEAD</code></pre><p>Then push your updated branch to the shared repository.</p><p><em>The commit history will show both commits, separately</em>.</p><hr><h2>Advanced: Correction of the <em>private</em> branch in public repository</h2><h3><em>This can be dangerous -- be sure you have a local copy of the branch to repush.</em></h3><p><em>Also note: You don't want to do this if someone else may be working on the branch.</em></p><pre><code>git push --delete (branch_name) ## remove public version of branch</code></pre><p>Clean up your branch locally then repush...</p><pre><code>git push origin (branch_name)</code></pre><p><em>In the normal case, you probably needn't worry about your private-branch commit history being pristine.  Just push a followup commit (see 'How to undo a public commit' above), and later, do a <a href=\"https://stackoverflow.com/a/22417539/86967\">squash-merge</a> to hide the history.</em></p>    ",
      "comments": [
        {
          "content": "<code>gitk --all $(git reflog | cut -c1-7)&amp;</code> may be helpful for finding the previous revision if you want to undo an &#39;--amend&#39; commit.",
          "date": "2014-10-18 23:38:11Z",
          "author": "Brent Bradburn",
          "reputation": "48783"
        },
        {
          "content": "It should be noted that if you&#39;re attempting to remove secret information before pushing to a shared repository, doing a revert won&#39;t help you, because the information will still be in the history in the previous commit.  If you want to ensure the change is never visible to others you need to use <code>git reset</code>",
          "date": "2015-09-04 04:52:01Z",
          "author": "Jherico",
          "reputation": "27818"
        },
        {
          "content": "Correcting a private branch in  remote repository can also be done by simply <code>git push origin (branch_name) --force</code>",
          "date": "2018-09-07 12:09:38Z",
          "author": "Brent Bradburn",
          "reputation": "48783"
        }
      ]
    },
    {
      "rating": "408",
      "date": "",
      "edited": "2021-04-10 09:04:56Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If you want to permanently undo it and you have cloned some repository.</p><p>The commit id can be seen by:</p><pre><code>git log </code></pre><p>Then you can do like:</p><pre><code>git reset --hard &lt;commit_id&gt;git push origin &lt;branch_name&gt; -f</code></pre>    ",
      "comments": [
        {
          "content": "What if you do not use &quot;&lt;commit_id&gt;&quot; and simply use &quot;git reset --hard&quot;? I typically just want to get rid of my latest updates that I have not committed yet and got back to the latest commit I made, and I always use &quot;git reset --hard&quot;.",
          "date": "2017-09-27 23:30:18Z",
          "author": "Jaime Montoya",
          "reputation": "6276"
        },
        {
          "content": "@JaimeMontoya To undo latest changes you can use <code>git reset --hard</code> , but if you have to hard remove last &quot;n&quot; commits you specify a SHA",
          "date": "2017-09-28 13:10:31Z",
          "author": "poorva",
          "reputation": "1686"
        }
      ]
    },
    {
      "rating": "403",
      "date": "",
      "edited": "2014-09-25 07:58:25Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If you have committed junk but not pushed,</p><pre><code>git reset --soft HEAD~1</code></pre><blockquote>  <p><strong>HEAD~1</strong> is a shorthand for the commit before head. Alternatively you can refer to the <strong>SHA-1</strong> of the hash if you want to reset to. <em>--soft</em> option will delete the commit but it will leave all your changed files \"Changes to be committed\", as git status would put it.</p>    <p>If you want to get rid of any changes to tracked files in the working tree since the commit before head use \"<strong>--hard</strong>\" instead.</p></blockquote><p>OR</p><blockquote>  <p>If you already pushed and someone pulled which is usually my case, you can't use <em>git reset</em>. You can however do a <em>git revert</em>,</p></blockquote><pre><code>git revert HEAD</code></pre><blockquote>  <p>This will create a new commit that reverses everything introduced by the accidental commit.</p></blockquote>    ",
      "comments": [
        {
          "content": "I&#39;m in the 2nd case, but when I do &quot;git revert HEAD&quot; it says &quot;error: Commit [ID] is a merge but no -m option was given.  fatal: revert failed&quot;.  Any suggestions?",
          "date": "2014-11-12 19:36:02Z",
          "author": "metaforge",
          "reputation": "881"
        },
        {
          "content": "Probably worth mentioning that instead of <code>HEAD~1</code> you could use the actual hash as displayed by <code>git log --stat</code> or by <code>git reflog</code> - useful when you need to &#39;undo&#39; more than one commit.",
          "date": "2014-12-07 00:38:49Z",
          "author": "ccpizza",
          "reputation": "26815"
        }
      ]
    },
    {
      "rating": "344",
      "date": "",
      "edited": "2015-06-24 09:34:11Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>On <a href=\"https://www.atlassian.com/software/sourcetree/overview\">SourceTree</a> (GUI for GitHub), you may right-click the commit and do a 'Reverse Commit'. This should undo your changes.</p><p>On the terminal:</p><p>You may alternatively use:</p><pre><code>git revert</code></pre><p>Or:</p><pre class=\"lang-bash prettyprint-override\"><code>git reset --soft HEAD^ # Use --soft if you want to keep your changes.git reset --hard HEAD^ # Use --hard if you don't care about keeping your changes.</code></pre>    ",
      "comments": []
    },
    {
      "rating": "316",
      "date": "",
      "edited": "2014-07-21 20:13:34Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>A single command:</p><pre class=\"lang-bash prettyprint-override\"><code>git reset --soft 'HEAD^' </code></pre><p>It works great to undo the last local commit!</p>    ",
      "comments": [
        {
          "content": "I needed to write git reset --soft &quot;HEAD^&quot; with double quotes, because I write it from Windows command prompt.",
          "date": "2014-04-23 09:13:35Z",
          "author": "Ena",
          "reputation": "3331"
        }
      ]
    },
    {
      "rating": "297",
      "date": "",
      "edited": "2017-10-27 12:46:34Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Just reset it doing the command below using <code>git</code>:</p><pre><code>git reset --soft HEAD~1</code></pre><p><strong>Explain:</strong> what <code>git reset</code> does, it's basically <code>reset</code> to any commit you'd like to go back to, then if you combine it with <code>--soft</code> key, it will go back, but keep the  changes in your file(s), so you get back to the stage which the file was just added, <code>HEAD</code> is the head of the branch and if you combine with <code>~1</code> (in this case you also use <code>HEAD^</code>), it will go back only one commit which what you want...</p><p>I create the steps in the image below in more details for you, including all steps that may happens in real situations and committing the code:</p><p><a href=\"https://i.stack.imgur.com/7zrzb.jpg\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/7zrzb.jpg\" alt=\"How to undo the last commits in Git?\"></a></p>    ",
      "comments": []
    },
    {
      "rating": "285",
      "date": "",
      "edited": "2022-02-16 15:57:24Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>&quot;Reset the working tree to the last commit&quot;</p><pre><code>git reset --hard HEAD^ </code></pre><p>&quot;Clean unknown files from the working tree&quot;</p><pre><code>git clean    </code></pre><p>see - <a href=\"https://web.archive.org/web/20210515091636/http://jonas.nitro.dk/git/quick-reference.html\" rel=\"noreferrer\">Git Quick Reference</a></p><p>NOTE: <strong>This command will delete your previous commit</strong>, so use with caution! <code>git reset --hard</code> is safer.</p>    ",
      "comments": [
        {
          "content": "What does <code>git reset --hard</code> do differently?",
          "date": "2022-04-28 22:01:25Z",
          "author": "Hashim Aziz",
          "reputation": "3160"
        }
      ]
    },
    {
      "rating": "274",
      "date": "",
      "edited": "2017-03-25 08:14:01Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>How to undo the last Git commit?</strong></p><p>To restore everything back to the way it was prior to the last commit, we need to reset to the commit before HEAD.</p><ol><li><p>If you don't want to keep your changes that you made:</p><pre><code>git reset --hard HEAD^</code></pre></li><li><p>If you want to keep your changes:</p><pre><code>git reset --soft HEAD^</code></pre></li></ol><p>Now check your git log. It will show that our last commit has been removed.</p>    ",
      "comments": []
    },
    {
      "rating": "221",
      "date": "2014-01-06 22:34:06Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Use reflog to find a correct state</p><pre><code>git reflog</code></pre><p><img src=\"https://i.stack.imgur.com/c7e7D.png\" alt=\"reflog before\"><em>REFLOG BEFORE RESET</em></p><p>Select the correct reflog (f3cb6e2 in my case) and type </p><pre><code>git reset --hard f3cb6e2</code></pre><p>After that the repo HEAD will be reset to that HEADid<img src=\"https://i.stack.imgur.com/GdnDT.png\" alt=\"reset effect\"><em>LOG AFTER RESET</em></p><p>Finally the reflog looks like the picture below</p><p><img src=\"https://i.stack.imgur.com/Fhhub.png\" alt=\"reflog after\"><em>REFLOG FINAL</em></p>    ",
      "comments": []
    },
    {
      "rating": "196",
      "date": "",
      "edited": "2015-06-24 09:33:20Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>First run: </p><pre><code>git reflog</code></pre><p>It will show you all the possible actions you have performed on your repository, for example, commit, merge, pull, etc.</p><p>Then do:</p><pre><code>git reset --hard ActionIdFromRefLog</code></pre>    ",
      "comments": []
    },
    {
      "rating": "182",
      "date": "",
      "edited": "2017-12-13 22:05:20Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h2>Undo last commit:</h2><p><code>git reset --soft HEAD^</code> or <code>git reset --soft HEAD~</code></p><p>This will undo the last commit.</p><p>Here <code>--soft</code> means reset into staging.</p><p><code>HEAD~</code> or <code>HEAD^</code> means to move to commit before HEAD.</p><hr><h2>Replace last commit to new commit:</h2><pre><code>git commit --amend -m \"message\"</code></pre><p>It will replace the last commit with the new commit.</p>    ",
      "comments": []
    },
    {
      "rating": "180",
      "date": "",
      "edited": "2015-06-24 09:34:21Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Another way:</p><p>Checkout the branch you want to revert, then reset your local working copy back to the commit that you want to be the latest one on the remote server (everything after it will go bye-bye). To do this, in SourceTree I right-clicked on the and selected \"Reset BRANCHNAME to this commit\".</p><p>Then navigate to your repository's local directory and run this command:</p><pre><code>git -c diff.mnemonicprefix=false -c core.quotepath=false push -v -f --tags REPOSITORY_NAME BRANCHNAME:BRANCHNAME</code></pre><p>This will erase all commits after the current one in your local repository but only for that one branch.</p>    ",
      "comments": []
    },
    {
      "rating": "168",
      "date": "",
      "edited": "2015-06-24 09:34:18Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Type <code>git log</code> and find the last commit hash code and then enter:</p><pre><code>git reset &lt;the previous co&gt;</code></pre>    ",
      "comments": []
    },
    {
      "rating": "163",
      "date": "",
      "edited": "2015-06-24 09:34:09Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In my case I accidentally committed some files I did not want to. So I did the following and it worked:</p><pre><code>git reset --soft HEAD^git rm --cached [files you do not need]git add [files you need]git commit -c ORIG_HEAD</code></pre><p>Verify the results with gitk or git log --stat</p>    ",
      "comments": []
    },
    {
      "rating": "158",
      "date": "",
      "edited": "2016-01-18 21:58:33Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Simple, run this in your command line:</p><pre><code>git reset --soft HEAD~ </code></pre>    ",
      "comments": []
    },
    {
      "rating": "155",
      "date": "",
      "edited": "2022-07-08 08:17:48Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h2>WHAT TO USE, <code>reset --soft</code> or <code>reset --hard</code>?</h2><p>I am just adding two cents for @Kyralessa's answer:</p><p>If you are unsure what to use go for <code>--soft</code> (I used this convention to remember it --<strong>s</strong>oft for safe).</p><h3>Why?</h3><p>If you choose <code>--hard</code> by mistake you will <strong>LOSE</strong> your changes as it wasn't before.If you choose <code>--soft</code> by mistake you can achieve the same results of <code>--hard</code> by applying additional commands</p><pre><code>git reset HEAD file.htmlgit checkout -- file.html</code></pre><h3>Full example</h3><pre><code>echo &quot;some changes...&quot; &gt; file.htmlgit add file.htmlgit commit -m &quot;wrong commit&quot;# I need to resetgit reset --hard HEAD~1 (cancel changes)# ORgit reset --soft HEAD~1 # Back to staginggit reset HEAD file.html # back to working directorygit checkout -- file.html # cancel changes</code></pre><p>Credits goes to @Kyralessa.</p>    ",
      "comments": [
        {
          "content": "The very useful description about differences <code>--soft</code> VS <code>--hard</code> <a href=\"https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting/\" rel=\"nofollow noreferrer\">atlassian.com/git/tutorials/&hellip;</a>",
          "date": "2016-12-15 16:29:21Z",
          "author": "Eugen Konkov",
          "reputation": "20020"
        },
        {
          "content": "One doesn&#39;t really lose the commits on a <code>--hard</code> reset as they will be available in the ref log for 30 days <code>git reflog</code>.",
          "date": "2017-09-11 14:10:59Z",
          "author": "Todd",
          "reputation": "622"
        }
      ]
    },
    {
      "rating": "148",
      "date": "",
      "edited": "2022-07-30 16:06:03Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>There are many ways to do it:</p><p>Git command to undo the last commit/ previous commits:</p><p><strong>Warning:</strong> Do Not use --hard if you do not know what you are doing.--hard is too <strong>dangerous</strong>, and it might <strong>delete your files.</strong></p><p><strong>Basic command to revert the commit in Git is:</strong></p><pre><code>$ git reset --hard &lt;COMMIT -ID&gt;</code></pre><p>or</p><pre><code>$ git reset --hard HEAD~&lt;n&gt;</code></pre><p><strong>COMMIT-ID</strong>: ID for the commit</p><p><strong>n:</strong>  is the number of last commits you want to revert</p><p>You can get the commit id as shown below:</p><pre><code>$ **git log --oneline**d81d3f1 function to subtract two numbersbe20eb8 function to add two numbersbedgfgg function to multiply two numbers</code></pre><p>where <strong>d81d3f1</strong> and <strong>be20eb8</strong> are commit id.</p><p><strong>Now, let's see some cases:</strong></p><p><em>Suppose you want to revert the last commit 'd81d3f1'.  Here are two options:</em></p><pre><code>$ git reset --hard d81d3f1</code></pre><p>or</p><pre><code>$ git reset --hard HEAD~1</code></pre><p><em>Suppose you want to revert the commit 'be20eb8':</em></p><pre><code>$ git reset --hard be20eb8</code></pre><p>For more detailed information, you can refer to and try out some other commands too for resetting the head to a specified state:</p><pre><code>$ git reset --help</code></pre>    ",
      "comments": [
        {
          "content": "<code>git reset --hard HEAD~1</code> is <b>too dangerous</b>! This will not just &#39;cancel last commit&#39;, but will revert repo completely back to the previous commit. So you will LOOSE all changes committed in the last commit!",
          "date": "2017-03-21 12:09:47Z",
          "author": "Arnis Juraga",
          "reputation": "1015"
        },
        {
          "content": "You right, to undo this you can use <code>git push -f &lt;remote&gt; HEAD@{1}:&lt;branch&gt;</code>",
          "date": "2017-04-24 13:07:03Z",
          "author": "Benny",
          "reputation": "2027"
        },
        {
          "content": "Unfortunately, I use --hard, and my files are deleted! I did not check the comment first because it is collapsed. Do not use --hard if you do not know what you are doing!",
          "date": "2018-08-19 13:53:17Z",
          "author": "anonymous",
          "reputation": "1242"
        },
        {
          "content": "I just all my code with git reset --hard HEAD~1, it does not bring it back to stage. It just deletes everything. Urgghh",
          "date": "2022-08-19 09:32:20Z",
          "author": "Prateek Mishra",
          "reputation": "1167"
        }
      ]
    },
    {
      "rating": "147",
      "date": "",
      "edited": "2016-01-11 23:50:47Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>There are two main scenarios</p><p><strong>You haven't pushed the commit yet</strong></p><p>If the problem was extra files you commited (and you don't want those on repository), you can remove them using <code>git rm</code> and then commiting with <code>--amend</code></p><pre><code>git rm &lt;pathToFile&gt;</code></pre><p>You can also remove entire directories with <code>-r</code>, or even combine with other <a href=\"http://en.wikipedia.org/wiki/Bash_%28Unix_shell%29\">Bash</a> commands</p><pre><code>git rm -r &lt;pathToDirectory&gt;git rm $(find -name '*.class')</code></pre><p>After removing the files, you can commit, with <strong>--amend</strong> option</p><pre><code>git commit --amend -C HEAD # the -C option is to use the same commit message</code></pre><p>This will rewrite your recent local commit removing the extra files, so, these files will never be sent on push and also will be removed from your local .git repository by GC.</p><p><strong>You already pushed the commit</strong></p><p>You can apply the same solution of the other scenario and then doing <code>git push</code> with the <code>-f</code> option, but it is <strong>not recommended</strong> since it overwrites the remote history with a divergent change (it can mess your repository).</p><p>Instead, you have to do the commit without <code>--amend</code> (remember this about -amend`: That option rewrites the history on the last commit).</p>    ",
      "comments": []
    },
    {
      "rating": "145",
      "date": "",
      "edited": "2014-07-21 20:15:08Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h3>For a local commit</h3><pre><code>git reset --soft HEAD~1</code></pre><p>or if you do not remember exactly in which commit it is, you might use</p><pre><code>git rm --cached &lt;file&gt;</code></pre><h3>For a pushed commit</h3><p>The proper way of removing files from the repository history is using <code>git filter-branch</code>. That is,</p><pre class=\"lang-bash prettyprint-override\"><code>git filter-branch --index-filter 'git rm --cached &lt;file&gt;' HEAD</code></pre><p>But I recomnend you use this command with care. Read more at <em><a href=\"https://www.kernel.org/pub/software/scm/git/docs/git-filter-branch.html\">git-filter-branch(1) Manual Page</a></em>.</p>    ",
      "comments": []
    },
    {
      "rating": "25017",
      "date": "",
      "edited": "2022-10-12 10:47:00Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "                <p>I accidentally committed the wrong files to <a href=\"https://en.wikipedia.org/wiki/Git\" rel=\"noreferrer\">Git</a>, but didn't push the commit to the server yet.</p><p>How do I undo those commits from the <em>local</em> repository?</p>    ",
      "comments": [
        {
          "content": "You know what git needs? <code>git undo</code>, that&#39;s it. Then the reputation git has for handling mistakes made by us mere mortals disappears. Implement by pushing the current state on a git stack before executing any <code>git</code> command. It would affect performance, so it would be best to add a config flag as to whether to enable it.",
          "date": "2018-03-20 01:45:28Z",
          "author": "Yimin Rong",
          "reputation": "1778"
        },
        {
          "content": "@YiminRong That can be done with Git&#39;s <code>alias</code> feature: <a href=\"https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases\" rel=\"nofollow noreferrer\">git-scm.com/book/en/v2/Git-Basics-Git-Aliases</a>",
          "date": "2018-10-05 14:50:08Z",
          "author": "Edric",
          "reputation": "22742"
        },
        {
          "content": "For VsCode users , just type ctrl +shift +G and then click on three dot ,ie , more options and then click on undo Last Commit",
          "date": "2019-04-08 12:15:40Z",
          "author": "ashad",
          "reputation": "351"
        },
        {
          "content": "@YiminRong Undo <i>what</i> exactly? There are dozens of very different functional cases where &quot;undoing&quot; means something <b>completely</b> different. I&#39;d bet adding a new fancy &quot;magic wand&quot; would only confuse things more.",
          "date": "2020-03-24 14:27:03Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        },
        {
          "content": "@YiminRong Not buying it. People would still fumble and undo things not to be undone. But more importantly, <code>git reflog</code> is already close to what you describe, but gives the user more control on what&#39;s to be (un)done. But please, no, &quot;undo&quot; does not work the same everywhere, and people would <i>expect</i> many different things for the feature to achieve. Undo last commit? Undo last action? If last action was a push, undo how exactly, (reset and push) or (revert and push)?",
          "date": "2020-03-25 13:23:33Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        }
      ]
    },
    {
      "rating": "141",
      "date": "",
      "edited": "2016-09-16 07:25:01Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>To reset to the previous revision, permanently deleting all uncommitted changes: </p><pre><code>git reset --hard HEAD~1</code></pre>    ",
      "comments": [
        {
          "content": "Maybe you could at a note/warning that his command will throw away the commit <b>and the changes in the working directory</b> without asking any further.",
          "date": "2014-11-24 22:35:29Z",
          "author": "cr7pt0gr4ph7",
          "reputation": "1234"
        },
        {
          "content": "If you happen to do this by accident, not all is lost, though. See <a href=\"http://stackoverflow.com/questions/10099258/how-can-i-recover-a-lost-commit-in-git\" title=\"how can i recover a lost commit in git\">stackoverflow.com/questions/10099258/&hellip;</a>, <a href=\"http://stackoverflow.com/questions/15479501/git-commit-lost-after-reset-hard-not-found-by-fsck-not-in-reflog\" title=\"git commit lost after reset hard not found by fsck not in reflog\">stackoverflow.com/questions/15479501/&hellip;</a> and <a href=\"http://stackoverflow.com/questions/7374069/undo-git-reset-hard/7376959\" title=\"undo git reset hard\">stackoverflow.com/questions/7374069/undo-git-reset-hard/7376959</a>.",
          "date": "2014-11-24 22:40:57Z",
          "author": "cr7pt0gr4ph7",
          "reputation": "1234"
        },
        {
          "content": "Use <code>--soft</code> to keep your changes as <code>uncommitted changes</code>, <code>--hard</code> to nuke the commit completely and revert back by one. Remember to do such operations only on changes, that are not pushed yet.",
          "date": "2015-03-09 09:11:06Z",
          "author": "Yunus Nedim Mehel",
          "reputation": "11887"
        },
        {
          "content": "@Zaz: You are right; maybe I should have clarified that. Only files/changes that have been either added to index (/staged) or have been committed can possibly be recovered. Uncommitted, unstaged changes <i>are</i>, as you said, completely thrown away by <code>git reset --hard</code>.",
          "date": "2016-09-13 21:17:10Z",
          "author": "cr7pt0gr4ph7",
          "reputation": "1234"
        },
        {
          "content": "As a sidenote: Everytime a file is staged, <code>git</code> stores its contents in its object database. The stored contents are only removed when garbage collection is executed. It is therefore possible to recover the last staged version of a file that was not currently staged when <code>git reset --hard</code> was executed (see the posts linked above for more information).",
          "date": "2016-09-13 21:22:52Z",
          "author": "cr7pt0gr4ph7",
          "reputation": "1234"
        }
      ]
    },
    {
      "rating": "133",
      "date": "",
      "edited": "2017-06-12 13:21:48Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Use <a href=\"https://www.atlassian.com/software/sourcetree/overview\" rel=\"noreferrer\">SourceTree</a> (graphical tool for Git) to see your commits and tree. You can manually reset it directly by right clicking it.</p>    ",
      "comments": []
    },
    {
      "rating": "98",
      "date": "",
      "edited": "2019-08-05 19:24:46Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Think we have <strong><em>code.txt</em></strong> file.We make some changes on it and commit.<strong>We can undo this commit in three ways</strong>, but first you should know what is the staged file...An staged file is a file that ready to commit and if you run <code>git status</code> this file will be shown with green color and if this is not staged for commit will be shown with red color:</p><p><a href=\"https://i.stack.imgur.com/ymNKl.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/ymNKl.png\" alt=\"enter image description here\"></a></p><p>It means if you commit your change, your changes on this file is not saved.You can add this file in your stage with <code>git add code.txt</code> and then commit your change:</p><p><a href=\"https://i.stack.imgur.com/36Yag.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/36Yag.png\" alt=\"enter image description here\"></a></p><p><strong>Undo last commit:</strong></p><ol><li><p>Now if we want to just undo commit without any other changes, we can use</p><p><code>git reset --soft HEAD^</code> </p><p><a href=\"https://i.stack.imgur.com/Tx6x1.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/Tx6x1.png\" alt=\"enter image description here\"></a></p></li><li><p>If we want to undo commit and its changes (<strong><em>THIS IS DANGEROUS, because your change will lost</em></strong>), we can use</p><p><code>git reset --hard HEAD^</code></p><p><a href=\"https://i.stack.imgur.com/8NDBS.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/8NDBS.png\" alt=\"enter image description here\"></a></p></li><li><p>And if we want to undo commit and remove changes from stage, we can use</p><p><code>git reset --mixed HEAD^</code> or in a short form <code>git reset HEAD^</code></p><p><a href=\"https://i.stack.imgur.com/jg8g0.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/jg8g0.png\" alt=\"enter image description here\"></a></p></li></ol>    ",
      "comments": []
    },
    {
      "rating": "86",
      "date": "",
      "edited": "2021-07-23 20:52:36Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Usually, you want to <strong>undo</strong> a commit because you made a mistake and you want to fix it - essentially what the OP did when he asked the question. Really, you actually want to <strong>redo</strong> a commit.</p><p>Most of the answers here focus on the command line. While the command line is the best way to use Git when you're comfortable with it, its probably a bit alien to those coming from other version control systems to Git.</p><p>Here's how to do it using a GUI. If you have Git installed, you already have everything you need to follow these instructions.</p><p><strong>NOTE:</strong> I will assume here that you realised the commit was wrong before you pushed it. If you don't know what pushing means, then you probably haven't pushed. Carry on with the instructions. If you have pushed the faulty commit, the least risky way is just to follow up the faulty commit with a new commit that fixes things, the way you would do it in a version control system that does not allow you to rewrite history.</p><p>That said, here's how to fix your most recent fault commit using a GUI:</p><ol><li>Navigate to your repository on the command line and start the GUI with <code>git gui</code></li><li>Choose &quot;Amend last commit&quot;. You will see your last commit message, the files you staged and the files you didn't.</li><li>Now change things to how you want them to look and click Commit.</li></ol>    ",
      "comments": []
    },
    {
      "rating": "80",
      "date": "",
      "edited": "2017-03-25 08:23:13Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can use:</p><pre><code>git reset HEAD@{1}</code></pre><p>This command will delete your wrong commit without a Git log.</p>    ",
      "comments": []
    },
    {
      "rating": "80",
      "date": "2021-08-18 12:35:37Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If you want to revert the last commit but still want to keep the changes locally that were made in the commit, use this command:</p><pre><code>git reset HEAD~1 --mixed</code></pre>    ",
      "comments": [
        {
          "content": "The right answer without the git tutorial.",
          "date": "2021-11-15 00:31:34Z",
          "author": "Ringo",
          "reputation": "4768"
        },
        {
          "content": "Simple and worked for me. I reset my password, logged in, hunted for this answer in the list since it was not in the same order, and voted you up.",
          "date": "2021-11-20 20:31:17Z",
          "author": "Brent Woodruff",
          "reputation": "81"
        }
      ]
    },
    {
      "rating": "75",
      "date": "",
      "edited": "2018-01-28 21:34:59Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>Undo the Last Commit</strong></p><p>There are tons of situations where you really want to undo that last commit into your code. E.g. because you'd like to restructure it extensively - or even discard it altogether!</p><p>In these cases, the \"reset\" command is your best friend:</p><pre><code>$ git reset --soft HEAD~1</code></pre><p>The above command (reset) will rewind your current HEAD branch to the specified revision. In our example above, we'd like to return to the one before the current revision - effectively making our last commit undone.</p><p>Note the <code>--soft</code> flag: this makes sure that the changes in undone revisions are preserved. After running the command, you'll find the changes as uncommitted local modifications in your working copy.</p><p>If you don't want to keep these changes, simply use the <code>--hard</code> flag. Be sure to only do this when you're sure you don't need these changes any more.</p><pre><code>$ git reset --hard HEAD~1</code></pre><p><a href=\"https://i.stack.imgur.com/hqetQ.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/hqetQ.png\" alt=\"Enter image description here\"></a></p>    ",
      "comments": [
        {
          "content": "&quot;Working copy&quot;? Is this a Git concept? Isn&#39;t it an SVN concept?",
          "date": "2018-01-28 21:36:56Z",
          "author": "Peter Mortensen",
          "reputation": "30613"
        },
        {
          "content": "@PeterMortensen yes working copy, its a git concept though",
          "date": "2018-05-04 19:46:45Z",
          "author": "Mohit",
          "reputation": "766"
        }
      ]
    },
    {
      "rating": "71",
      "date": "",
      "edited": "2015-05-18 16:55:34Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Just undo the last commit: </p><pre><code>git reset --soft HEAD~</code></pre><p>Or undo the time before last time commit: </p><pre><code>git reset --soft HEAD~2</code></pre><p>Or undo any previous commit: </p><pre><code>git reset --soft &lt;commitID&gt;</code></pre><p>(you can get the commitID using <code>git reflog</code>)</p><p>When you undo a previous commit, remember to clean the workplace with</p><pre><code>git clean</code></pre><p>More details can be found in the docs: <a href=\"http://git-scm.com/docs/git-reset\">git-reset</a></p>    ",
      "comments": []
    },
    {
      "rating": "68",
      "date": "",
      "edited": "2019-10-14 14:42:44Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Before answering let's add some background, explaining what is this <code>HEAD</code>.</p><h1><strong><em><code>First of all what is HEAD?</code></em></strong></h1><p><code>HEAD</code> is simply a reference to the current commit (latest) on the current branch.<br>There can only be a single <code>HEAD</code> at any given time. (excluding <code>git worktree</code>)</p><p>The content of <code>HEAD</code> is stored inside <code>.git/HEAD</code> and it contains the 40 bytes SHA-1 of the current commit.</p><hr><h1><strong><em><code>detached HEAD</code></em></strong></h1><p>If you are not on the latest commit - meaning that <code>HEAD</code> is pointing to a prior commit in history its called <strong><em><code>detached HEAD</code></em></strong>.</p><p><a href=\"https://i.stack.imgur.com/OlavO.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/OlavO.png\" alt=\"enter image description here\"></a></p><p>On the command line, it will look like this- SHA-1 instead of the branch name since the <code>HEAD</code> is not pointing to the tip of the current branch</p><p><a href=\"https://i.stack.imgur.com/qplvo.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/qplvo.png\" alt=\"enter image description here\"></a></p><h2><a href=\"https://i.stack.imgur.com/U0l3s.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/U0l3s.png\" alt=\"enter image description here\"></a></h2><h3>A few options on how to recover from a detached HEAD:</h3><hr><h3><a href=\"https://git-scm.com/docs/git-checkout\" rel=\"noreferrer\"><code>git checkout</code></a></h3><pre><code>git checkout &lt;commit_id&gt;git checkout -b &lt;new branch&gt; &lt;commit_id&gt;git checkout HEAD~X // x is the number of commits t go back</code></pre><p>This will checkout new branch pointing to the desired commit.<br>This command will checkout to a given commit.<br>At this point, you can create a branch and start to work from this point on.</p><pre><code># Checkout a given commit. # Doing so will result in a `detached HEAD` which mean that the `HEAD`# is not pointing to the latest so you will need to checkout branch# in order to be able to update the code.git checkout &lt;commit-id&gt;# create a new branch forked to the given commitgit checkout -b &lt;branch name&gt;</code></pre><hr><h3><a href=\"https://git-scm.com/docs/git-reflog\" rel=\"noreferrer\"><code>git reflog</code></a></h3><p>You can always use the <code>reflog</code> as well.<br><code>git reflog</code> will display any change which updated the <code>HEAD</code> and checking out the desired reflog entry will set the <code>HEAD</code> back to this commit. </p><p><strong>Every time the HEAD is modified there will be a new entry in the <code>reflog</code></strong></p><pre><code>git refloggit checkout HEAD@{...}</code></pre><p>This will get you back to your desired commit</p><p><a href=\"https://i.stack.imgur.com/atW9w.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/atW9w.png\" alt=\"enter image description here\"></a></p><hr><h3><strong><em><a href=\"https://git-scm.com/docs/git-reset\" rel=\"noreferrer\"><code>git reset --hard &lt;commit_id&gt;</code></a></em></strong></h3><p>\"Move\" your HEAD back to the desired commit.</p><pre class=\"lang-sh prettyprint-override\"><code># This will destroy any local modifications.# Don't do it if you have uncommitted work you want to keep.git reset --hard 0d1d7fc32# Alternatively, if there's work to keep:git stashgit reset --hard 0d1d7fc32git stash pop# This saves the modifications, then reapplies that patch after resetting.# You could get merge conflicts if you've modified things which were# changed since the commit you reset to.</code></pre><ul><li>Note: (<a href=\"https://github.com/git/git/blob/master/Documentation/RelNotes/2.7.0.txt\" rel=\"noreferrer\">Since Git 2.7</a>)<br>you can also use the <code>git rebase --no-autostash</code> as well.</li></ul><hr><h3><strong><em><a href=\"https://git-scm.com/docs/git-revert\" rel=\"noreferrer\"><code>git revert &lt;sha-1&gt;</code></a></em></strong></h3><p>\"Undo\" the given commit or commit range.<br>The reset command will \"undo\" any changes made in the given commit.<br>A new commit with the undo patch will be committed while the original commit will remain in the history as well.</p><pre class=\"lang-sh prettyprint-override\"><code># add new commit with the undo of the original one.# the &lt;sha-1&gt; can be any commit(s) or commit rangegit revert &lt;sha-1&gt;</code></pre><hr><p>This schema illustrates which command does what.<br>As you can see there <code>reset &amp;&amp; checkout</code> modify the <code>HEAD</code>.</p><p><a href=\"https://i.stack.imgur.com/NuThL.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/NuThL.png\" alt=\"enter image description here\"></a></p>    ",
      "comments": []
    },
    {
      "rating": "65",
      "date": "",
      "edited": "2021-07-23 21:09:24Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In my case I committed and pushed to the wrong branch, so what I wanted was to have all my changes back so I can commit them to a new correct branch, so I did this:</p><p>On the same branch that you committed and pushed, if you type &quot;git status&quot; you won't see anything new because you committed and pushed, now type:</p><pre><code>    git reset --soft HEAD~1</code></pre><p>This will get all your changes(files) back in the stage area, now to get them back in the working directory(unstage) you just type:</p><pre><code>git reset FILE</code></pre><p>Where &quot;File&quot; is the file that you want to commit again. Now, this FILE should be in the working directory(unstaged) with all the changes that you did. Now you can change to whatever branch that you want and commit the changes in that branch.  Of course, the initial branch that you committed is still there with all changes, but in my case that was ok, if it is not for you-you can look for ways to revert that commit in that branch.</p>    ",
      "comments": []
    },
    {
      "rating": "64",
      "date": "",
      "edited": "2019-03-13 05:18:36Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>Undo the last commit:</strong></p><pre><code>git reset --soft HEAD^ or git reset --soft HEAD~</code></pre><p><strong>This will undo the last commit.</strong></p><p>Here <code>--soft</code> means reset into staging.</p><p><code>HEAD~ or HEAD^</code> means to move to commit before HEAD.</p><p>Replace the last commit to new commit:</p><pre><code>git commit --amend -m \"message\"</code></pre><p>It will replace the last commit with the new commit.</p>    ",
      "comments": []
    },
    {
      "rating": "61",
      "date": "",
      "edited": "2017-03-25 09:22:19Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If you are working with <strong><a href=\"https://www.sourcetreeapp.com/\" rel=\"noreferrer\">SourceTree</a></strong>, this will help you.</p><p><strong>Right click</strong> on the commit then <strong>select</strong> \"<em>Reset (current branch)/master to this commit</em>\" and last <strong>select</strong>  <em>\"Soft\" reset</em>.</p><p><a href=\"https://i.stack.imgur.com/BSMo2.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/BSMo2.png\" alt=\"Enter image description here\"></a></p>    ",
      "comments": []
    },
    {
      "rating": "59",
      "date": "",
      "edited": "2018-11-10 09:26:01Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>To undo your local commit you use <code>git reset &lt;commit&gt;</code>. Also <a href=\"https://www.atlassian.com/git/tutorials/undoing-changes/git-reset\" rel=\"noreferrer\">that tutorial</a> is very helpful to show you how it works.</p><p>Alternatively, you can use <code>git revert &lt;commit&gt;</code>: <a href=\"https://www.atlassian.com/git/tutorials/undoing-changes/git-revert/\" rel=\"noreferrer\">reverting</a> should be used when you want to add another commit that rolls back the changes (but keeps them in the project history).</p>    ",
      "comments": [
        {
          "content": "Be <b>extra</b> careful when reverting merge commits. You may lose your commits. Read about what Linus says about that: <a href=\"https://www.kernel.org/pub/software/scm/git/docs/howto/revert-a-faulty-merge.html\" rel=\"nofollow noreferrer\">kernel.org/pub/software/scm/git/docs/howto/&hellip;</a>",
          "date": "2016-12-15 16:25:41Z",
          "author": "Eugen Konkov",
          "reputation": "20020"
        },
        {
          "content": "Note that for git reset &lt;commit&gt;, use the commit sha for the PREVIOUS commit that you want to reset to, not the commit that you want to unstage.",
          "date": "2022-07-07 15:25:33Z",
          "author": "Rich G",
          "reputation": "265"
        }
      ]
    },
    {
      "rating": "58",
      "date": "",
      "edited": "2019-12-13 16:40:28Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Suppose you made a wrong commit locally and pushed it to a remote repository. You can undo the mess with these two commands:</p><p>First, we need to correct our local repository by going back to the commit that we desire:</p><pre><code>git reset --hard &lt;previous good commit id where you want the local repository  to go&gt;</code></pre><p>Now we forcefully push this good commit on the remote repository by using this command:</p><pre><code>git push --force-with-lease</code></pre><p>The 'with-lease' version of the force option it will prevent accidental deletion of new commits you do not know about (i.e. coming from another source since your last pull).</p>    ",
      "comments": [
        {
          "content": "this worked for me the best, since I had already pushed the bad commit up to github",
          "date": "2019-05-06 20:36:37Z",
          "author": "AeroHil",
          "reputation": "817"
        }
      ]
    },
    {
      "rating": "52",
      "date": "",
      "edited": "2017-03-25 08:22:44Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>VISUAL STUDIO USERS (2015, etc.)</strong></p><p>If you cannot synchronise in Visual Studio as you are not allowed to push to a branch like \"development\" then as much as I tried, in Visual Studio NEITHER the <strong>REVERT</strong> NOR the <strong>RESET</strong> (hard or soft) would work.</p><p>Per the answer with TONS OF VOTES:</p><p>Use this at the command prompt of root of your project to nuke anything that will attempt to get pushed:</p><pre><code>git reset --hard HEAD~1</code></pre><p>Backup or zip your files just in case you don't wish to lose any work, etc...</p>    ",
      "comments": []
    },
    {
      "rating": "52",
      "date": "",
      "edited": "2021-01-18 20:44:51Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Everybody comments in such a complicated manner.</p><p>If you want to remove the last commit from your branch, the simplest way to do it is:</p><pre><code>git reset --hard HEAD~1</code></pre><p>Now to actually push that change to get rid of your last commit, you have to</p><pre><code>git push --force</code></pre><p>And that's it. This will remove your last commit.</p>    ",
      "comments": [
        {
          "content": "But keep in mind that --hard will completely discard all the changes that were made in the last commit as well as the content of the index.",
          "date": "2020-03-14 11:58:44Z",
          "author": "CloudJR",
          "reputation": "53"
        },
        {
          "content": "Yes, in the case we need to just fix the commit, we shoud not use <code>--hard`, but leave it with the default </code>--soft` so we can remove and add stuff before the ``git push -f`",
          "date": "2021-03-11 16:29:35Z",
          "author": "Maf",
          "reputation": "596"
        }
      ]
    },
    {
      "rating": "51",
      "date": "",
      "edited": "2020-06-20 09:12:55Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h3>A Typical Git Cycle</h3><p>In speaking of Git-related commands in the previous answers, I would like to share my typical Git cycles with all readers which may helpful. Here is how I work with Git,</p><ol><li><p>Cloning the first time from the remote server</p><p><code>git clone $project</code></p></li><li><p>Pulling from remote (when I don't have a pending local commit to push)</p><p><code>git pull</code></p></li><li><p>Adding a new local file1 into $to_be_committed_list (just imagine $to_be_committed_list means <code>staged</code> area)</p><p><code>git add $file1</code></p></li><li><p>Removing mistakenly added file2 from $to_be_committed_list (assume that file2 is added like step 3, which I didn't want)</p><p><code>git reset $file2</code></p></li><li><p>Committing file1 which is in $to_be_committed_list</p><p><code>git commit -m &quot;commit message description&quot;</code></p></li><li><p>Syncing local commit with remote repository before pushing</p><p><code>git pull --rebase</code></p></li><li><p>Resolving when conflict occurs <a href=\"https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration\" rel=\"noreferrer\">prerequisite configure mergetool</a></p><p><code>git mergetool #resolve merging here, also can manually merge</code></p></li><li><p>Adding conflict-resolved files, let's say <code>file1</code>:</p><p><code>git add $file1</code></p></li><li><p>Continuing my previous rebase command</p><p><code>git rebase --continue</code></p></li><li><p>Pushing ready and already synced last local commit</p><p><code>git push origin head:refs/for/$branch # branch = master, dev, etc.</code></p></li></ol>    ",
      "comments": [
        {
          "content": "What if I am working on a fork, so basically I have 2 remotes actual repo e.g. incubator-mxnet and my forked repo ChaiBapchya/incubator-mxnet  So in such a case, how can I solve merge conflicts from local to my forked repo branch",
          "date": "2018-10-30 19:18:27Z",
          "author": "Chaitanya Bapat",
          "reputation": "2833"
        }
      ]
    },
    {
      "rating": "48",
      "date": "",
      "edited": "2020-06-17 09:54:00Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In these cases, the \"reset\" command is your best friend:</p><pre><code>git reset --soft HEAD~1</code></pre><p>Reset will rewind your current HEAD branch to the specified revision. In our example above, we'd like to return to the one before the current revision - effectively making our last commit undone.</p><p>Note the --soft flag: this makes sure that the changes in undone revisions are preserved. After running the command, you'll find the changes as uncommitted local modifications in your working copy.</p><p>If you don't want to keep these changes, simply use the --hard flag. Be sure to only do this when you're sure you don't need these changes anymore.</p><pre><code>git reset --hard HEAD~1</code></pre>    ",
      "comments": []
    },
    {
      "rating": "46",
      "date": "",
      "edited": "2019-03-13 05:22:12Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>Remove a wrong commit that is already pushed to Github</strong></p><pre><code>git push origin +(previous good commit id):(branch name)</code></pre><p>Please specify the last good commit id you would like to reset back in Github.</p><p>For example. If latest commit id is wrong then specify the previous commit id in above git command with the branch name. </p><p>You can get previous commit id using <code>git log</code></p>    ",
      "comments": []
    },
    {
      "rating": "46",
      "date": "",
      "edited": "2021-07-23 21:15:17Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In order to get rid of (all the changes in) last commit, last 2 commits and last n commits:</p><pre><code>git reset --hard HEAD~1git reset --hard HEAD~2...git reset --hard HEAD~n</code></pre><p>And, to get rid of anything after a specific commit:</p><pre><code>git reset --hard &lt;commit sha&gt;</code></pre><p>e.g.,</p><pre><code>git reset --hard 0d12345</code></pre><p>Be careful with the hard option: it deletes the local changes in your repo as well and reverts to the previous mentioned commit. You should only run this if you are sure you messed up in your last commit(s) and would like to go back in time.</p><p>As a side-note, about 7 letters of the commit hash is enough, but in bigger projects, you may need up to 12 letters for it to be unique. You can also use the entire commit SHA if you prefer.</p><p>The above commands work in GitHub for Windows as well.</p>    ",
      "comments": []
    },
    {
      "rating": "41",
      "date": "",
      "edited": "2017-03-25 08:14:49Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You need to do the easy and fast</p><pre><code>    git commit --amend</code></pre><p>if it's a private branch or</p><pre><code>    git commit -m 'Replace .class files with .java files'</code></pre><p>if it's a shared or public branch.</p>    ",
      "comments": []
    },
    {
      "rating": "41",
      "date": "",
      "edited": "2017-03-25 08:21:11Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>I got the commit ID from <code>bitbucket</code> and then did:</p><pre><code>git checkout commitID .</code></pre><p>Example:</p><pre><code>git checkout 7991072 .</code></pre><p>And it reverted it back up to that working copy of that commit.</p>    ",
      "comments": [
        {
          "content": "Note: checking out &#39;5456cea9&#39;.  You are in &#39;detached HEAD&#39; state. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by performing another checkout.  If you want to create a new branch to retain commits you create, you may do so (now or later) by using -b with the checkout command again. Example:    git checkout -b &lt;new-branch-name&gt;  HEAD is now at 5456cea... Need to delete Exclusions.xslt from Documentation folder. - Delete What should i do after this",
          "date": "2019-05-22 14:02:25Z",
          "author": "cSharma",
          "reputation": "636"
        }
      ]
    },
    {
      "rating": "35",
      "date": "",
      "edited": "2021-07-23 21:57:14Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Do as the following steps.</p><p><strong>Step 1</strong></p><p>Hit <code>git log</code></p><p>From the list of log, find the last commit hash code and then enter:</p><p><strong>Step 2</strong></p><pre><code>git reset &lt;hash code&gt;</code></pre>    ",
      "comments": [
        {
          "content": "After this, how can I fix the wrong commit to exclude wrong files?",
          "date": "2021-03-11 16:23:35Z",
          "author": "Maf",
          "reputation": "596"
        }
      ]
    },
    {
      "rating": "33",
      "date": "2015-07-06 08:30:20Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Use this command</p><pre><code>git checkout -b old-state 0d1d7fc32</code></pre>    ",
      "comments": []
    },
    {
      "rating": "31",
      "date": "",
      "edited": "2017-03-25 09:24:02Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Use this command:</p><pre><code>git checkout -b old-state number_commit</code></pre>    ",
      "comments": []
    },
    {
      "rating": "31",
      "date": "",
      "edited": "2018-11-10 09:27:48Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can always do a <code>git checkout &lt;SHA code&gt;</code>  of the previous version and then commit again with the new code.</p>    ",
      "comments": []
    },
    {
      "rating": "30",
      "date": "",
      "edited": "2021-06-23 22:33:05Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In order to remove some files from a Git commit, use the “git reset” command with the “–soft” option and specify the commit before HEAD.</p><pre><code>$ git reset --soft HEAD~1</code></pre><p>When running this command, you will be presented with the files from the most recent commit (HEAD) and you will be able to commit them.</p><p>Now that your files are in the staging area, you can remove them (or unstage them) using the “git reset” command again.</p><pre><code>$ git reset HEAD &lt;file&gt;</code></pre><p>Note: this time, you are resetting from HEAD as you simply want to exclude files from your staging area</p><p>If you are simply not interested in this file any more, you can use the “git rm” command in order to delete the file from the index (also called the staging area).</p><pre><code>$ git rm --cached &lt;file&gt;</code></pre><p>When you are done with the modifications, you can simply commit your changes again with the “–amend” option.</p><pre><code>$ git commit --amend</code></pre><p>To verify that the files were correctly removed from the repository, you can run the “git ls-files” command and check that the file does not appear in the file (if it was a new one of course)</p><pre><code>$ git ls-files &lt;file1&gt; &lt;file2&gt;</code></pre><h3>Remove a File From a Commit using Git Restore</h3><p>Since Git 2.23, there is a new way to remove files from commit, but you will have to make sure that you are using a Git version greater or equal than 2.23.</p><pre><code>$ git --version</code></pre><p>Git version 2.24.1</p><p>Note: Git 2.23 was released in August 2019 and you may not have this version already available on your computer.</p><p>To install newer versions of Git, you can check this tutorial.To remove files from commits, use the “git restore” command, specify the source using the “–source” option and the file to be removed from the repository.</p><p>For example, in order to remove the file named “myfile” from the HEAD, you would write the following command</p><pre><code>$ git restore --source=HEAD^ --staged  -- &lt;file&gt;</code></pre><p>As an example, let’s pretend that you edited a file in your most recent commit on your “master” branch.</p><p>The file is correctly committed but you want to remove it from your Git repository.</p><p>To remove your file from the Git repository, you want first to restore it.</p><pre><code>$ git restore --source=HEAD^ --staged  -- newfile$ git status</code></pre><h3>On branch 'master'</h3><p>Your branch is ahead of 'origin/master' by 1 commit.(use &quot;git push&quot; to publish your local commits)</p><p>Changes to be committed:</p><pre><code> (use &quot;git restore --staged &lt;file&gt;...&quot; to unstage)    modified:   newfile</code></pre><p>Changes not staged for commit:</p><pre><code> (use &quot;git add &lt;file&gt;...&quot; to update what will be committed) (use &quot;git restore &lt;file&gt;...&quot; to discard changes in working directory)      modified:   newfile</code></pre><p>As you can see, your file is back to the staging area.</p><p>From there, you have two choices, you can choose to edit your file in order to re-commit it again, or to simply delete it from your Git repository.</p><h3>Remove a File from a Git Repository</h3><p>In this section, we are going to describe the steps in order to remove the file from your Git repository.</p><p>First, you need to unstage your file as you won’t be able to remove it if it is staged.</p><p>To unstage a file, use the “git reset” command and specify the HEAD as source.</p><pre><code>$ git reset HEAD newfile</code></pre><p>When your file is correctly unstaged, use the “git rm” command with the “–cached” option in order to remove this file from the Git index (this won’t delete the file on disk)</p><pre><code>$ git rm --cached newfile</code></pre><p>rm 'newfile'</p><p>Now if you check the repository status, you will be able to see that Git staged a deletion commit.</p><pre><code>$ git status</code></pre><h3>On branch 'master'</h3><p>Your branch is ahead of 'origin/master' by 1 commit.(use &quot;git push&quot; to publish your local commits)</p><p>Changes to be committed:</p><pre><code> (use &quot;git restore --staged &lt;file&gt;...&quot; to unstage)    deleted:    newfile</code></pre><p>Now that your file is staged, simply use the “git commit” with the “–amend” option in order to amend the most recent commit from your repository.</p><pre><code>`$ git commit --amend [master 90f8bb1] Commit from HEAD  Date: Fri Dec 20 03:29:50 2019 -0500  1 file changed, 2 deletions(-)  delete mode 100644 newfile</code></pre><p>`As you can see, this won’t create a new commit but it will essentially modify the most recent commit in order to include your changes.</p><h3>Remove a Specific File from a Git Commit</h3><p>In some cases, you don’t want all the files to be staged again: you only one to modify one very specific file of your repository.</p><p>In order to remove a specific file from a Git commit, use the “git reset” command with the “–soft” option, specify the commit before HEAD and the file that you want to remove.</p><pre><code>$ git reset HEAD^ -- &lt;file&gt;</code></pre><p>When you are done with the modifications, your file will be back in the staging area.</p><p>First, you can choose to remove the file from the staging area by using the “git reset” command and specify that you want to reset from the HEAD.</p><pre><code>$ git reset HEAD &lt;file&gt;</code></pre><p>Note: it does not mean that you will lose the changes on this file, just that the file will be removed from the staging area.</p><p>If you want to completely remove the file from the index, you will have to use the “git rm” command with the “–cached” option.</p><pre><code>$ git reset HEAD &lt;file&gt;</code></pre><p>In order to make sure that your file was correctly removed from the staging area, use the “git ls-files” command to list files that belong to the index.</p><pre><code>$ git ls-files</code></pre><p>When you are completely done with your modifications, you can amend the commit you removed the files from by using the “git commit” command with the “–amend” option.</p><pre><code>$ git commit --amend</code></pre>    ",
      "comments": [
        {
          "content": "What&#39;s the difference between using <code>git rm file` and </code>git checkout file`?",
          "date": "2021-03-11 16:37:26Z",
          "author": "Maf",
          "reputation": "596"
        }
      ]
    },
    {
      "rating": "29",
      "date": "",
      "edited": "2018-10-19 18:10:45Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can undo your Git commits in two ways:First is you can use <code>git revert</code>, if you want to keep your commit history:</p><pre><code>git revert HEAD~3git revert &lt;hashcode of commit&gt;</code></pre><p>Second is you can use <code>git reset</code>, which would delete all your commit history and bring your head to commit where you want it.</p><pre><code>git reset &lt;hashcode of commit&gt;git reset HEAD~3</code></pre><p>You can also use the <code>--hard</code> keyword if any of it starts behaving otherwise. But, I would only recommend it until it's extremely necessary.</p>    ",
      "comments": []
    },
    {
      "rating": "29",
      "date": "",
      "edited": "2019-07-10 20:25:50Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre><code>git reset --soft HEAD~1</code></pre><p>Reset will rewind your current HEAD branch to the specified revision.</p><p><strong>Note</strong> the <code>--soft</code> flag: this makes sure that the changes in undone revisions are preserved. After running the command, you'll find the changes as uncommitted local modifications in your working copy.</p><p>If you don't want to keep these changes, simply use the <code>--hard</code> flag. Be sure to only do this when you're sure you don't need these changes anymore.</p><pre><code> git reset --hard HEAD~1</code></pre><blockquote>  <p>Undoing Multiple Commits</p></blockquote><pre><code>git reset --hard 0ad5a7a6</code></pre><p>Keep in mind, however, that using the reset command undoes all commits that came after the one you returned to:</p><p><a href=\"https://i.stack.imgur.com/slheG.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/slheG.png\" alt=\"Enter image description here\"></a></p>    ",
      "comments": []
    },
    {
      "rating": "27",
      "date": "",
      "edited": "2018-10-19 18:07:24Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can undo your commits from the local repository. Please follow the below scenario.</p><p>In the below image I check out the 'test' branch (using Git command <code>git checkout -b test</code>) as a local and check status (using Git command <code>git status</code>) of local branch that there is nothing to commit.</p><p><a href=\"https://i.stack.imgur.com/kQ0Ds.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/kQ0Ds.png\" alt=\"Enter image description here\"></a></p><p>In the next image image you can see here I made a few changes in <strong>Filter1.txt</strong> and added that file to the staging area and then committed my changes with some message (using Git command <code>git commit -m \"Doing commit to test revert back\"</code>).</p><p><strong>\"-m is for commit message\"</strong></p><p><a href=\"https://i.stack.imgur.com/OpWzO.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/OpWzO.png\" alt=\"Enter image description here\"></a></p><p>In the next image you can see your commits log whatever you have made commits (using Git command <code>git log</code>).</p><p><a href=\"https://i.stack.imgur.com/2keAh.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/2keAh.png\" alt=\"Enter image description here\"></a></p><p>So in the above image you can see the commit id with each commit and with your commit message now whatever commit you want to revert back or undo copy that commit id and hit the below Git command,<code>git revert {\"paste your commit id\"}</code>. Example:</p><pre><code>git revert 9ca304ed12b991f8251496b4ea452857b34353e7</code></pre><p><a href=\"https://i.stack.imgur.com/mi04t.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/mi04t.png\" alt=\"Enter image description here\"></a></p><p>I have reverted back my last commit. Now if you check your Git status, you can see the modified file which is <strong>Filter1.txt</strong> and yet to commit.</p><p><a href=\"https://i.stack.imgur.com/dfdMu.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/dfdMu.png\" alt=\"Enter image description here\"></a></p>    ",
      "comments": []
    },
    {
      "rating": "25017",
      "date": "",
      "edited": "2022-10-12 10:47:00Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "                <p>I accidentally committed the wrong files to <a href=\"https://en.wikipedia.org/wiki/Git\" rel=\"noreferrer\">Git</a>, but didn't push the commit to the server yet.</p><p>How do I undo those commits from the <em>local</em> repository?</p>    ",
      "comments": [
        {
          "content": "You know what git needs? <code>git undo</code>, that&#39;s it. Then the reputation git has for handling mistakes made by us mere mortals disappears. Implement by pushing the current state on a git stack before executing any <code>git</code> command. It would affect performance, so it would be best to add a config flag as to whether to enable it.",
          "date": "2018-03-20 01:45:28Z",
          "author": "Yimin Rong",
          "reputation": "1778"
        },
        {
          "content": "@YiminRong That can be done with Git&#39;s <code>alias</code> feature: <a href=\"https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases\" rel=\"nofollow noreferrer\">git-scm.com/book/en/v2/Git-Basics-Git-Aliases</a>",
          "date": "2018-10-05 14:50:08Z",
          "author": "Edric",
          "reputation": "22742"
        },
        {
          "content": "For VsCode users , just type ctrl +shift +G and then click on three dot ,ie , more options and then click on undo Last Commit",
          "date": "2019-04-08 12:15:40Z",
          "author": "ashad",
          "reputation": "351"
        },
        {
          "content": "@YiminRong Undo <i>what</i> exactly? There are dozens of very different functional cases where &quot;undoing&quot; means something <b>completely</b> different. I&#39;d bet adding a new fancy &quot;magic wand&quot; would only confuse things more.",
          "date": "2020-03-24 14:27:03Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        },
        {
          "content": "@YiminRong Not buying it. People would still fumble and undo things not to be undone. But more importantly, <code>git reflog</code> is already close to what you describe, but gives the user more control on what&#39;s to be (un)done. But please, no, &quot;undo&quot; does not work the same everywhere, and people would <i>expect</i> many different things for the feature to achieve. Undo last commit? Undo last action? If last action was a push, undo how exactly, (reset and push) or (revert and push)?",
          "date": "2020-03-25 13:23:33Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        }
      ]
    },
    {
      "rating": "27",
      "date": "",
      "edited": "2018-10-19 18:08:18Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Here is site: <a href=\"http://ohshitgit.com/\" rel=\"noreferrer\"><code>Oh shit, git!</code></a>.</p><p>Here are many recipes how to undo things in Git. Some of them:</p><blockquote>  <p>Oh shit, I need to change the message on my last commit!</p></blockquote><pre><code>git commit --amend# follow prompts to change the commit message</code></pre><blockquote>  <p>Oh shit, I accidentally committed something to master that should have been on a brand new branch!</p></blockquote><pre><code># Create a new branch from the current state of mastergit branch some-new-branch-name# Remove the commit from the master branchgit reset HEAD~ --hardgit checkout some-new-branch-name# Your commit lives in this branch now :)</code></pre>    ",
      "comments": []
    },
    {
      "rating": "26",
      "date": "",
      "edited": "2018-11-10 09:16:05Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>I have found <a href=\"http://ohshitgit.com/\" rel=\"noreferrer\">this</a> site which describes how to undo things that you have committed into the repository.</p><p>Some commands:</p><pre><code>git commit --amend        # Change last commitgit reset HEAD~1 --soft   # Undo last commit</code></pre>    ",
      "comments": []
    },
    {
      "rating": "26",
      "date": "",
      "edited": "2022-01-23 22:51:37Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Reference: <a href=\"https://web.archive.org/web/20210414042516/http://www.coding-issues.com/2017/03/how-to-undo-or-revert-last-commits-in-git.html\" rel=\"noreferrer\">How to undo last commit in Git?</a></p><p>If you have Git Extensions installed you can easily undo/revert any commit (you can download Git Extensions from <a href=\"https://gitextensions.github.io\" rel=\"noreferrer\">here</a>).</p><p>Open Git Extensions, right click on the commit you want to revert then select &quot;Revert commit&quot;.</p><p><a href=\"https://i.stack.imgur.com/j2fK1.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/j2fK1.png\" alt=\"Git Extensions screen shot\" /></a></p><p>A popup will be opened (see the screenshot below)</p><p><a href=\"https://i.stack.imgur.com/Skufo.jpg\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/Skufo.jpg\" alt=\"Revert commit popup\" /></a></p><p>Select &quot;Automatically create a commit&quot; if you want to directly commit the reverted changes or if you want to manually commit the reverted changes keep the box un-selected and click on &quot;Revert this commit&quot; button.</p>    ",
      "comments": []
    },
    {
      "rating": "25",
      "date": "2018-12-10 07:05:41Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>The simplest way to undo the last commit is</p><pre><code>git reset HEAD^</code></pre><p>This will bring the project state before you have made the commit.</p>    ",
      "comments": []
    },
    {
      "rating": "25",
      "date": "2018-12-21 02:02:27Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h1>The difference between git reset --mixed, --soft and --hard</h1><blockquote>  <p>Prerequisite: When a modification to an existing file in your  repository is made, this change is initially considered as unstaged.  In order to commit the changes, it needs to be staged which means  adding it to the index using <code>git add</code>. During a commit operation, the  files that are staged gets added to an index.</p></blockquote><p>Let's take an example: </p><pre><code>- A - B - C (master)</code></pre><p><code>HEAD</code> points to <code>C</code> and the index matches <code>C</code>.</p><h2>--soft</h2><ul><li>When we execute <code>git reset --soft B</code> with the intention of <strong>removing the commit C</strong> and <strong>pointing the master/HEAD to B</strong>. </li><li>The master/HEAD will now point to B, but the <strong>index still has changed from C</strong>. </li><li>When executing <code>git status</code> you could see the files indexed in <strong>commit C</strong> as <strong>staged</strong>. </li><li>Executing a <code>git commit</code> at this point will create a new commit with <strong>the same changes as C</strong></li></ul><h2>--mixed</h2><ul><li>Execute <code>git reset --mixed B</code>. </li><li>On execution, master/HEAD will point to B and the <strong>index is also modified to match B</strong> because of the mixed flag used.  </li><li>If we run git commit at this point, nothing will happen since the <strong>index matches HEAD</strong>.</li><li>We still have the changes in the working directory, but since they're not in the index, <strong>git status shows them as unstaged</strong>. </li><li>To commit them, you would <code>git add</code> and then commit as usual.</li></ul><h2>--hard</h2><ul><li>Execute <code>git reset --hard B</code></li><li>On execution, master/HEAD will point to B <strong>and modifies your working directory</strong></li><li>The <strong>changes added in C</strong> and <strong>all the uncommitted changes</strong> will be <strong>removed</strong>.</li><li>Files in the working copy will match the commit B, this will result in loosing permanently all changes which were made in commit C plus uncommitted changes</li></ul><p>Hope this comparison of flags that are available to use with <code>git reset</code> command will help someone to use them wisely. Refer these for further details <a href=\"https://gist.github.com/tnguyen14/0827ae6eefdff39e452b\" rel=\"noreferrer\">link1</a> &amp; <a href=\"https://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard\">link2</a></p>    ",
      "comments": []
    },
    {
      "rating": "25",
      "date": "",
      "edited": "2022-04-26 23:17:23Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>For sake of completeness, I will give the one glaringly obvious method that was overlooked by the previous answers.</p><p>Since the commit was not pushed, the remote was unchanged, so:</p><ol><li>Delete the local repository.</li><li>Clone the remote repository.</li></ol><p>This is sometimes necessary if your fancy Git client goes bye-bye (looking at you, egit).</p><p>Don't forget to re-commit your <em>saved</em> changes since the last push.</p>    ",
      "comments": []
    },
    {
      "rating": "24",
      "date": "2021-10-05 13:26:47Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h3>Undoing a series of local commits</h3><blockquote><p>OP: How do I undo the most recent local commits in Git? I accidentally committed the wrong files [as part of several commits].</p></blockquote><h4>Start case</h4><p>There are several ways to &quot;undo&quot; as series of commits, depending on the outcome you're after. Considering the start case below, <code>reset</code>, <code>rebase</code> and <code>filter-branch</code> can all be used to rewrite your history.</p><p><a href=\"https://i.stack.imgur.com/CtqYJ.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/CtqYJ.png\" alt=\"Star-case\" /></a></p><p><em><strong>How can <em>C1</em> and <em>C2</em> be undone to remove the <code>tmp.log</code> file from each commit?</strong></em></p><p>In the examples below, absolute commit references are used, but it works the same way if you're more used to relative references (i.e. <code>HEAD~2</code> or <code>HEAD@{n}</code>).</p><h4>Alternative 1: <code>reset</code></h4><pre><code>$ git reset --soft t56pi</code></pre><p><a href=\"https://i.stack.imgur.com/UEmaO.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/UEmaO.png\" alt=\"using-reset\" /></a></p><p>With <code>reset</code>, a branch can be reset to a previous state, and any compounded changes be reverted to the <em>Staging Area</em>, from where any unwanted changes can then be discarded.</p><p><strong>Note:</strong> As <code>reset</code> clusters all previous changes into the <em>Staging Area</em>, individual commit meta-data is lost. If this is not OK with you, chances are you're probably better off with <code>rebase</code> or <code>filter-branch</code> instead.</p><h4>Alternative 2: <code>rebase</code></h4><pre><code>$ git rebase --interactive t56pi</code></pre><p><a href=\"https://i.stack.imgur.com/h4IuR.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/h4IuR.png\" alt=\"using-rebase\" /></a></p><p>Using an interactive <code>rebase</code> each offending commit in the branch can be rewritten, allowing you to modify and discard unwanted changes. In the infographic above, the source tree on the right illustrates the state post <code>rebase</code>.</p><p><strong>Step-by-step</strong></p><ol><li>Select from which commit the rebase should be based (e.g. <code>t56pi</code>)</li><li>Select which commits you'd like to change by replacing <code>pick</code> with <code>edit</code>. Save and close.</li><li>Git will now stop on each selected commit allowing you to reset <code>HEAD</code>, remove the unwanted files, and create brand new commits.</li></ol><p><strong>Note:</strong> With <code>rebase</code> much of the commit meta data is kept, in contrast to the <code>reset</code> alternative above. This is most likely a preferred option, if you want to keep much of your history but only remove the unwanted files.</p><h4>Alternative 3: <code>filter-branch</code></h4><pre><code>$ git filter-branch --tree-filter 'rm -r ./tmp.log' t56pi..HEAD</code></pre><p>Above command would filter out the file <code>./tmp.log</code> from all commits in the desired range <code>t56pi..HEAD</code> (assuming our initial start case from above). See below illustration for clarity.</p><p><a href=\"https://i.stack.imgur.com/K0CSV.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/K0CSV.png\" alt=\"using-filter-branch\" /></a></p><p>Similar to <code>rebase</code>, <code>filter-branch</code> can be used to wipe unwanted files from a subsection of a branch. Instead of manually editing each commit through the rebase process, <code>filter-branch</code> can automatically preformed the desired action on each commit.</p><p><strong>Note:</strong> Just like <code>rebase</code>, <code>filter-branch</code> would preserve the rest of the commit meta-data, by only discarding the desired file. Notice how <em>C1</em> and <em>C2</em> have been rewritten, and the log-file discarded from each commit.</p><h4>Conclusion</h4><p>Just like anything related to software development, there are multiple ways to achieve the same (or similar) outcome for a give problem. You just need to pick the one most suitable for your particular case.</p><h4>Finally - a friendly advice</h4><p>Do note that all three alternatives above rewrites the history completely. Unless you know exactly what you're doing and have good communication within your team - only rewrite commits that have not yet been published remotely!</p><p><em><strong>Source:</strong></em> All examples above are borrowed from this <a href=\"https://blog.git-init.com/\" rel=\"noreferrer\">blog</a>.</p>    ",
      "comments": []
    },
    {
      "rating": "23",
      "date": "",
      "edited": "2018-10-19 18:13:03Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>HEAD:</strong></p><p>Before reset commit we should know about HEAD... HEAD is nothing but your current state in your working directory. It is represented by a commit number.</p><p><strong>Git commit:</strong></p><p>Each change assigned under a commit which is represented by a unique tag. Commits can't be deleted. So if you want your last commit, you can simply dive into it using <code>git reset</code>.</p><p>You can dive into the last commit using two methods:</p><p><strong>Method 1:</strong> (if you don't know the commit number, but want to move onto the very first)</p><pre><code>git reset HEAD~1  # It will move your head to last commit</code></pre><p><strong>Method 2:</strong> (if you know the commit you simply reset onto your known commit)</p><p><code>git reset 0xab3</code> # Commit number</p><p><strong>Note:</strong> if you want to know a recent commit try <code>git log -p -1</code></p><p>Here is the graphical representation:</p><p><a href=\"https://i.stack.imgur.com/boiKO.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/boiKO.png\" alt=\"Enter image description here\"></a></p>    ",
      "comments": []
    },
    {
      "rating": "22",
      "date": "2016-05-29 14:21:17Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Just use <code>git reset --hard &lt;last good SHA&gt;</code> to reset your changes and give new commit. You can also use <code>git checkout -- &lt;bad filename&gt;</code>.</p>    ",
      "comments": []
    },
    {
      "rating": "22",
      "date": "",
      "edited": "2021-07-23 22:00:25Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>I validate an efficient method proposed, and here is a concrete example using it:</p><p>In case you want to permanently undo/cancel your last commit (and so on, one by one, as many as you want) three steps:</p><p>1: Get the id = SHA of the commit you want to arrive on with, of course</p><pre><code>$ git log</code></pre><p>2: Delete your previous commit with</p><pre><code>$ git reset --hard 'your SHA'</code></pre><p>3: Force the new local history upon your origin GitHub with the <code>-f</code> option (the last commit track will be erased from the GitHub history)</p><pre><code>$ git push origin master -f</code></pre><h3 id=\"example-c416\">Example</h3><pre><code>$ git log</code></pre><p>Last commit to cancel</p><pre><code>commit e305d21bdcdc51d623faec631ced72645cca9131 (HEAD -&gt; master, origin/master, origin/HEAD)Author: Christophe &lt;blabla@bla.com&gt;Date:   Thu Jul 30 03:42:26 2020 +0200U2_30 S45; updating files package.json &amp; yarn.lock for GitHub Web Page from docs/CV_Portfolio...</code></pre><p>Commit we want now on HEAD</p><pre><code>commit 36212a48b0123456789e01a6c174103be9a11e61Author: Christophe &lt;blabla@bla.com&gt;Date:   Thu Jul 30 02:38:01 2020 +0200First commit, new title</code></pre><h3 id=\"reach-the-commit-before-by-deleting-the-last-one-v99t\">Reach the commit before by deleting the last one</h3><pre><code>$ git reset --hard 36212a4HEAD is now at 36212a4 First commit, new title</code></pre><h3 id=\"check-its-ok-4t0c\">Check it's OK</h3><pre><code>$ git logcommit 36212a48b0123456789e01a6c174103be9a11e61 (HEAD -&gt; master)Author: Christophe &lt;blabla@bla.com&gt;Date:   Thu Jul 30 02:38:01 2020 +0200    First commit, new title$ git statusOn branch masterYour branch is behind 'origin/master' by 1 commit, and can be fast-forwarded. (use &quot;git pull&quot; to update your local branch)nothing to commit, working tree clean</code></pre><h3 id=\"update-your-history-on-the-github-tf3t\">Update your history on the Git(Hub)</h3><pre><code>$ git push origin master -fTotal 0 (delta 0), reused 0 (delta 0), pack-reused 0To https://github.com/ GitUser bla bla/React-Apps.git + e305d21...36212a4 master -&gt; master (forced update)</code></pre><h3 id=\"check-its-all-right-2xnf\">Check it's all right</h3><pre><code>$ git statusOn branch masterYour branch is up to date with 'origin/master'.nothing to commit, working tree clean</code></pre>    ",
      "comments": []
    },
    {
      "rating": "21",
      "date": "2021-09-26 17:50:46Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Before answering, let's add some background, explaining what this <code>HEAD</code> is.</p><h1><em><strong><code>First of all what is HEAD?</code></strong></em></h1><p><code>HEAD</code> is simply a reference to the current commit (latest) on the current branch. <br/>There can only be a single <code>HEAD</code> at any given time (excluding <code>git worktree</code>).</p><p>The content of <code>HEAD</code> is stored inside <code>.git/HEAD</code> and it contains the 40 bytes SHA-1 of the current commit.</p><hr /><h1><em><strong><code>detached HEAD</code></strong></em></h1><p>If you are not on the latest commit - meaning that <code>HEAD</code> is pointing to a prior commit in history it's called <em><strong><code>detached HEAD</code></strong></em>.</p><p><a href=\"https://i.stack.imgur.com/OlavO.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/OlavO.png\" alt=\"Enter image description here\" /></a></p><p>On the command line, it will look like this - SHA-1 instead of the branch name since the <code>HEAD</code> is not pointing to the tip of the current branch:</p><p><a href=\"https://i.stack.imgur.com/qplvo.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/qplvo.png\" alt=\"Enter image description here\" /></a></p><p><a href=\"https://i.stack.imgur.com/U0l3s.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/U0l3s.png\" alt=\"Enter image description here\" /></a></p><hr /><h3>A few options on how to recover from a detached HEAD:</h3><hr /><h3><a href=\"https://git-scm.com/docs/git-checkout\" rel=\"noreferrer\"><code>git checkout</code></a></h3><pre><code>git checkout &lt;commit_id&gt;git checkout -b &lt;new branch&gt; &lt;commit_id&gt;git checkout HEAD~X // x is the number of commits to go back</code></pre><p>This will checkout the new branch pointing to the desired commit. <br/>This command will checkout to a given commit. <br/>At this point, you can create a branch and start to work from this point on.</p><pre><code># Checkout a given commit.# Doing so will result in a `detached HEAD` which mean that the `HEAD`# is not pointing to the latest so you will need to checkout branch# in order to be able to update the code.git checkout &lt;commit-id&gt;# Create a new branch forked to the given commitgit checkout -b &lt;branch name&gt;</code></pre><hr /><h3><a href=\"https://git-scm.com/docs/git-reflog\" rel=\"noreferrer\"><code>git reflog</code></a></h3><p>You can always use the <code>reflog</code> as well. <br/><code>git reflog </code> will display any change which updated the <code>HEAD</code> and checking out the desired reflog entry will set the <code>HEAD</code> back to this commit.</p><p><strong>Every time the HEAD is modified there will be a new entry in the <code>reflog</code></strong></p><pre><code>git refloggit checkout HEAD@{...}</code></pre><p>This will get you back to your desired commit</p><p><a href=\"https://i.stack.imgur.com/atW9w.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/atW9w.png\" alt=\"Enter image description here\" /></a></p><hr /><h3><em><strong><a href=\"https://git-scm.com/docs/git-reset\" rel=\"noreferrer\"><code>git reset --hard &lt;commit_id&gt;</code></a></strong></em></h3><p>&quot;Move&quot; your HEAD back to the desired commit.</p><pre class=\"lang-sh prettyprint-override\"><code># This will destroy any local modifications.# Don't do it if you have uncommitted work you want to keep.git reset --hard 0d1d7fc32# Alternatively, if there's work to keep:git stashgit reset --hard 0d1d7fc32git stash pop# This saves the modifications, then reapplies that patch after resetting.# You could get merge conflicts if you've modified things which were# changed since the commit you reset to.</code></pre><ul><li>Note: (<a href=\"https://github.com/git/git/blob/master/Documentation/RelNotes/2.7.0.txt\" rel=\"noreferrer\">Since Git 2.7</a>) you can also use the <code>git rebase --no-autostash</code> as well.</li></ul><p><a href=\"https://i.stack.imgur.com/iyaGj.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/iyaGj.png\" alt=\"enter image description here\" /></a></p><hr /><h3><em><strong><a href=\"https://git-scm.com/docs/git-revert\" rel=\"noreferrer\"><code>git revert &lt;sha-1&gt;</code></a></strong></em></h3><p>&quot;Undo&quot; the given commit or commit range. <br/>The reset command will &quot;undo&quot; any changes made in the given commit. <br/>A new commit with the undo patch will be committed while the original commit will remain in history as well.</p><pre class=\"lang-sh prettyprint-override\"><code># Add a new commit with the undo of the original one.# The &lt;sha-1&gt; can be any commit(s) or commit rangegit revert &lt;sha-1&gt;</code></pre><hr /><p>This schema illustrates which command does what. <br/>As you can see there, <code>reset &amp;&amp; checkout</code> modify the <code>HEAD</code>.</p><p><a href=\"https://i.stack.imgur.com/NuThL.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/NuThL.png\" alt=\"Enter image description here\" /></a></p>    ",
      "comments": []
    },
    {
      "rating": "20",
      "date": "",
      "edited": "2021-06-23 22:14:44Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre class=\"lang-sh prettyprint-override\"><code>git reset --soft HEAD~1git status</code></pre><h3>Output</h3><blockquote><p>On branch master <br />Your branch is ahead of 'origin/master' by 1 commit. <br />(use &quot;git push&quot; to publish your local commits)</p><p>Changes to be committed: <br />(use &quot;git restore --staged ...&quot; to unstage)new file:   file1</p></blockquote><pre class=\"lang-sh prettyprint-override\"><code>git log --oneline --graph</code></pre><h3>Output</h3><blockquote><ul><li>90f8bb1 (HEAD -&gt; master) Second commit \\</li><li>7083e29 Initial repository commit \\</li></ul></blockquote>    ",
      "comments": []
    },
    {
      "rating": "20",
      "date": "",
      "edited": "2021-07-23 21:15:37Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If you would like to eliminate the wrong files you should do</p><p><code>git reset --soft &lt;your_last_good_commit_hash_here&gt;</code>Here, if you do <code>git status</code>, you will see the files in the staging area. You can select the wrong files and take them down from the staging area.</p><p>Like the following.</p><p><code>git reset wrongFile1 wrongFile2 wrongFile3</code></p><p>You can now just add the files that you need to push,</p><p><code>git add goodFile1 goodFile2</code></p><p>Commit them</p><p><code>git commit -v</code> or <code>git commit -am &quot;Message&quot;</code></p><p>And push</p><p><code>git push origin master</code></p><p>However, if you do not care about the changed files you can hard reset to previous good commit and push everything to the server.</p><p>By</p><pre><code>git reset --hard &lt;your_last_good_commit_hash_here&gt;</code></pre><p><code>git push origin master</code></p><p>If you already published your wrong files to the server, you can use the <code>--force</code> flag to push to the server and edit the history.</p><p><code>    git push --force origin master</code></p>    ",
      "comments": []
    },
    {
      "rating": "18",
      "date": "",
      "edited": "2018-07-02 18:21:13Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Rebasing and dropping commits are the best when you want to keep the history cleanuseful when proposing patches to a public branch etc.</p><p>If you have to drop the topmost commit then the following one-liner helps</p><pre><code>git rebase --onto HEAD~1 HEAD</code></pre><p>But if you want to drop 1 of many commits you did say</p><p>a -> b -> c -> d -> master</p><p>and you want to drop commit 'c'</p><pre><code>git rebase --onto b c</code></pre><p>This will make 'b' as the new base of 'd' eliminating 'c'</p>    ",
      "comments": []
    },
    {
      "rating": "18",
      "date": "",
      "edited": "2018-10-19 18:15:09Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In IntelliJ IDEA you can just open the Git repository log by pressing <kbd>Alt</kbd>+<kbd>9</kbd>, right mouse button click at some tag from the commits list, and select: <em>\"Reset Current Branch to Here...\"</em>.</p>    ",
      "comments": []
    },
    {
      "rating": "17",
      "date": "",
      "edited": "2021-07-23 21:56:28Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>What I do each time I need to undo a commit/commits are:</p><ol><li><p><code>git reset HEAD~&lt;n&gt;</code>  // the number of last commits I need to undo</p></li><li><p><code>git status</code> // optional. All files are now in red (unstaged).</p></li><li><p>Now, I can add &amp; commit just the files that I need:</p></li></ol><ul><li><code>git add &lt;file names&gt; &amp; git commit -m &quot;message&quot; -m &quot;details&quot;</code></li></ul><ol start=\"4\"><li>Optional: I can rollback the changes of the rest files, if I need, to their previous condition, with checkout:</li></ol><ul><li><code>git checkout &lt;filename&gt;</code></li></ul><ol start=\"5\"><li>if I had already pushed it to remote origin, previously:</li></ol><ul><li><code>git push origin &lt;branch name&gt; -f</code> // use -f to force the push.</li></ul>    ",
      "comments": []
    },
    {
      "rating": "16",
      "date": "2019-10-23 09:19:37Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>To undo the last local commit, without throwing away its changes, I have this handy alias in <code>~/.gitconfig</code></p><pre><code>[alias]  undo = reset --soft HEAD^</code></pre><p>Then I simply use <code>git undo</code> which is super-easy to remember.</p>    ",
      "comments": []
    },
    {
      "rating": "15",
      "date": "",
      "edited": "2018-10-19 18:15:49Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Find the last commit hash code by seeing the log by:</p><pre><code>git log</code></pre><p>Then</p><pre><code>git reset &lt;the previous co&gt;</code></pre>    ",
      "comments": []
    },
    {
      "rating": "14",
      "date": "",
      "edited": "2022-10-12 07:52:09Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><a href=\"https://i.stack.imgur.com/ueFVY.png\" rel=\"nofollow noreferrer\"><img src=\"https://i.stack.imgur.com/ueFVY.png\" alt=\"enter image description here\" /></a></p><p>Assuming you're working in Visual Studio, if you go in to your branch history and look at all of your commits, simply select the event prior to the commit you want to undo, right-click it, and select <code>Revert</code>.  Easy as that.</p>    ",
      "comments": []
    },
    {
      "rating": "13",
      "date": "",
      "edited": "2019-07-10 20:27:46Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Replace your local version, including your changes with the server version. These two lines of code will force Git to pull and overwrite local.</p><p>Open a command prompt and navigate to the Git project root. If you use Visual Studio, click on <em>Team</em>, <em>Sync</em> and click on \"Open Command Prompt\" (see the image) below.</p><p><a href=\"https://i.stack.imgur.com/sezIw.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/sezIw.png\" alt=\"Visual Studio\"></a></p><p>Once in the Cmd prompt, go ahead with the following two instructions.</p><pre><code>git fetch --all</code></pre><p>Then you do</p><pre><code>git reset --hard origin/master</code></pre><p>This will overwrite the existing local version with the one on the Git server.</p>    ",
      "comments": []
    },
    {
      "rating": "13",
      "date": "",
      "edited": "2021-05-27 23:13:06Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>I wrote about this ages ago after having these same problems myself:</p><p><em><a href=\"https://ao.ms/how-to-delete-revert-a-git-commit/\" rel=\"nofollow noreferrer\">How to delete/revert a Git commit</a></em></p><p>Basically you just need to do:</p><p><code>git log</code>, get the first seven characters of the SHA hash, and then do a <code>git revert &lt;sha&gt;</code> followed by <code>git push --force</code>.</p><p>You can also revert this by using the Git revert command as follows: <code>git revert &lt;sha&gt; -m -1</code> and then <code>git push</code>.</p>    ",
      "comments": []
    },
    {
      "rating": "12",
      "date": "2018-04-17 15:41:15Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Try this, hard reset to previous commit where those files were not added, then:</p><pre><code>git reset --hard &lt;commit_hash&gt;</code></pre><p>Make sure you have a backup of your changes just in case, as it's a hard reset, which means they'll be lost (unless you stashed earlier)</p>    ",
      "comments": []
    },
    {
      "rating": "12",
      "date": "",
      "edited": "2021-07-23 21:56:32Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If you simply want to trash all your local changes/commits and make your local branch look like the origin branch you started from...</p><pre><code>git reset --hard origin/branch-name</code></pre>    ",
      "comments": []
    },
    {
      "rating": "11",
      "date": "",
      "edited": "2018-10-03 03:07:54Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h2>If you want to undo the very first commit in your repo</h2><p>You'll encounter this problem:</p><pre><code>$ git reset HEAD~fatal: ambiguous argument 'HEAD~': unknown revision or path not in the working tree.Use '--' to separate paths from revisions, like this:'git &lt;command&gt; [&lt;revision&gt;...] -- [&lt;file&gt;...]'</code></pre><p>The error occurs because if the last commit is the initial commit (or no parents) of the repository, there is no HEAD~.</p><h2>Solution</h2><p><strong>If you want to reset the only commit on \"master\" branch</strong></p><pre><code>$ git update-ref -d HEAD$ git rm --cached -r .</code></pre>    ",
      "comments": []
    },
    {
      "rating": "10",
      "date": "",
      "edited": "2018-12-06 22:00:44Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Get last commit ID by using this command (in the log one on the top it is the latest one):</p><pre><code>git log</code></pre><p>Get the commit id (GUID) and run this command:</p><pre><code>git revert &lt;commit_id&gt;</code></pre>    ",
      "comments": [
        {
          "content": "What is the difference with @JacoPretorius answer ?",
          "date": "2018-11-21 13:05:23Z",
          "author": "R&#xE9;mi P",
          "reputation": "412"
        }
      ]
    },
    {
      "rating": "8",
      "date": "",
      "edited": "2019-07-10 20:24:51Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can use <code>git revert &lt;commit-id&gt;</code>.</p><p>And for getting the commit ID, just use <code>git log</code>.</p>    ",
      "comments": []
    },
    {
      "rating": "8",
      "date": "2021-06-11 21:58:37Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Visual Studio Code makes this very easy.</p><p><a href=\"https://i.stack.imgur.com/QXTef.png\" rel=\"noreferrer\"><img src=\"https://i.stack.imgur.com/QXTef.png\" alt=\"VS Code\" /></a></p>    ",
      "comments": [
        {
          "content": "And this wasn&#39;t covered by one of the previous 151 answers (not a rhetorical question)? (Including deleted answers). In any case, you could make an (annotated) list of links to similar answers (as 152 answers (including deleted answer) as 6 long, long pages makes it difficult to navigate) to make navigation easier - say the ones that contain solutions using Visual Studio Code. (<a href=\"https://pmortensen.eu/EditOverflow/_Wordlist/EditOverflowList_latest.html\" rel=\"nofollow noreferrer\">30 synonyms for Visual Studio Code have been observed in the wild</a> so far.)",
          "date": "2021-06-23 22:21:22Z",
          "author": "Peter Mortensen",
          "reputation": "30613"
        },
        {
          "content": "No, it was not.",
          "date": "2021-06-24 20:49:26Z",
          "author": "Kyle Delaney",
          "reputation": "11181"
        }
      ]
    },
    {
      "rating": "8",
      "date": "2021-07-16 05:16:20Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre><code>#1) $ git commit -m &quot;Something terribly misguided&quot; #2) $ git reset HEAD~                              </code></pre><p>[ edit files as necessary ]</p><pre><code> #3) $ git add .#4) $ git commit -c ORIG_HEAD  </code></pre>    ",
      "comments": []
    },
    {
      "rating": "7",
      "date": "",
      "edited": "2018-09-26 10:33:27Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre><code>git revert commit</code></pre><p>This will generate the opposite changes from the commit which you want to revert back, and then just commit that changes. I think this is the simplest way.</p><p><a href=\"https://git-scm.com/docs/git-revert\" rel=\"noreferrer\">https://git-scm.com/docs/git-revert</a></p>    ",
      "comments": []
    },
    {
      "rating": "7",
      "date": "",
      "edited": "2018-10-20 12:30:29Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<h3>How to <em>edit</em> an earlier commit</h3><p>Generally I don't want to undo a bunch of commits, but rather edit an earlier commit to how I wish I had committed it in the first place.</p><p>I found myself fixing a past commit frequently enough that I wrote a script for it.</p><p>Here's the workflow:</p><ol><li><pre><code>git commit-edit &lt;commit-hash&gt;</code></pre><p>This will drop you at the commit you want to edit.</p><p>The changes of the commit will be <strong>un</strong>staged, ready to be staged as you wish it was the first time.</p></li><li><p>Fix and stage the commit as you wish it had been in the first place.</p><p>(You may want to use <code>git stash save --keep-index</code> to squirrel away any files you're not committing)</p></li><li><p>Redo the commit with <code>--amend</code>, eg:</p><pre><code>git commit --amend</code></pre></li><li><p>Complete the rebase:</p><pre><code>git rebase --continue</code></pre></li></ol><hr><p>Call this following <code>git-commit-edit</code> and put it in your <code>$PATH</code>:</p><pre class=\"lang-bash prettyprint-override\"><code>#!/bin/bash# Do an automatic git rebase --interactive, editing the specified commit# Revert the index and working tree to the point before the commit was staged# https://stackoverflow.com/a/52324605/5353461set -euo pipefailscript_name=${0##*/}warn () { printf '%s: %s\\n' \"$script_name\" \"$*\" &gt;&amp;2; }die () { warn \"$@\"; exit 1; }[[ $# -ge 2 ]] &amp;&amp; die \"Expected single commit to edit. Defaults to HEAD~\"# Default to editing the parent of the most recent commit# The most recent commit can be edited with `git commit --amend`commit=$(git rev-parse --short \"${1:-HEAD~}\")# Be able to show what commit we're editing to the userif git config --get alias.print-commit-1 &amp;&gt;/dev/null; then  message=$(git print-commit-1 \"$commit\")else  message=$(git log -1 --format='%h %s' \"$commit\")fiif [[ $OSTYPE =~ ^darwin ]]; then  sed_inplace=(sed -Ei \"\")else  sed_inplace=(sed -Ei)fiexport GIT_SEQUENCE_EDITOR=\"${sed_inplace[*]} \"' \"s/^pick ('\"$commit\"' .*)/edit \\\\1/\"'git rebase --quiet --interactive --autostash --autosquash \"$commit\"~git reset --quiet @~ \"$(git rev-parse --show-toplevel)\"  # Reset the cache of the toplevel directory to the previous commitgit commit --quiet --amend --no-edit --allow-empty  #  Commit an empty commit so that that cache diffs are un-reversedechoecho \"Editing commit: $message\" &gt;&amp;2echo</code></pre>    ",
      "comments": []
    },
    {
      "rating": "25017",
      "date": "",
      "edited": "2022-10-12 10:47:00Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "                <p>I accidentally committed the wrong files to <a href=\"https://en.wikipedia.org/wiki/Git\" rel=\"noreferrer\">Git</a>, but didn't push the commit to the server yet.</p><p>How do I undo those commits from the <em>local</em> repository?</p>    ",
      "comments": [
        {
          "content": "You know what git needs? <code>git undo</code>, that&#39;s it. Then the reputation git has for handling mistakes made by us mere mortals disappears. Implement by pushing the current state on a git stack before executing any <code>git</code> command. It would affect performance, so it would be best to add a config flag as to whether to enable it.",
          "date": "2018-03-20 01:45:28Z",
          "author": "Yimin Rong",
          "reputation": "1778"
        },
        {
          "content": "@YiminRong That can be done with Git&#39;s <code>alias</code> feature: <a href=\"https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases\" rel=\"nofollow noreferrer\">git-scm.com/book/en/v2/Git-Basics-Git-Aliases</a>",
          "date": "2018-10-05 14:50:08Z",
          "author": "Edric",
          "reputation": "22742"
        },
        {
          "content": "For VsCode users , just type ctrl +shift +G and then click on three dot ,ie , more options and then click on undo Last Commit",
          "date": "2019-04-08 12:15:40Z",
          "author": "ashad",
          "reputation": "351"
        },
        {
          "content": "@YiminRong Undo <i>what</i> exactly? There are dozens of very different functional cases where &quot;undoing&quot; means something <b>completely</b> different. I&#39;d bet adding a new fancy &quot;magic wand&quot; would only confuse things more.",
          "date": "2020-03-24 14:27:03Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        },
        {
          "content": "@YiminRong Not buying it. People would still fumble and undo things not to be undone. But more importantly, <code>git reflog</code> is already close to what you describe, but gives the user more control on what&#39;s to be (un)done. But please, no, &quot;undo&quot; does not work the same everywhere, and people would <i>expect</i> many different things for the feature to achieve. Undo last commit? Undo last action? If last action was a push, undo how exactly, (reset and push) or (revert and push)?",
          "date": "2020-03-25 13:23:33Z",
          "author": "Romain Valeri",
          "reputation": "18262"
        }
      ]
    },
    {
      "rating": "7",
      "date": "",
      "edited": "2021-01-18 20:38:37Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre><code>$ git commit -m 'Initial commit'$ git add forgotten_file$ git commit --amend</code></pre><p>It’s important to understand that when you’re amending your last commit, you’re not so much fixing it as replacing it entirely with a new, improved commit that pushes the old commit out of the way and puts the new commit in its place. Effectively, it’s as if the previous commit never happened, and it won’t show up in your repository history.</p><p>The obvious value to amending commits is to make minor improvements to your last commit, without cluttering your repository history with commit messages of the form, “Oops, forgot to add a file” or “Darn, fixing a typo in last commit”.</p><p><em><a href=\"https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things\" rel=\"nofollow noreferrer\">2.4 Git Basics - Undoing Things</a></em></p>    ",
      "comments": [
        {
          "content": "What if we wanted to revert a single file?",
          "date": "2021-03-11 16:31:16Z",
          "author": "Maf",
          "reputation": "596"
        }
      ]
    },
    {
      "rating": "6",
      "date": "",
      "edited": "2021-01-18 20:28:14Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>I usually first find the commit hash of my recent commit:</p><pre><code>git log</code></pre><p>It looks like this: <code>commit {long_hash}</code></p><p>Copy this <code>long_hash</code> and reset to it (go back to same files/state it was on that commit):</p><pre><code>git reset --hard {insert long_hash without braces}</code></pre>    ",
      "comments": [
        {
          "content": "Notice the ``--hard option` will discard all changes not allowing us to discard only some files",
          "date": "2021-03-11 16:38:56Z",
          "author": "Maf",
          "reputation": "596"
        }
      ]
    },
    {
      "rating": "5",
      "date": "2018-05-18 07:00:35Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre><code>git push --delete (branch_name) //this will be removing the public version of your branchgit push origin (branch_name) //This will add the previous version back</code></pre>    ",
      "comments": []
    },
    {
      "rating": "5",
      "date": "2022-06-17 18:12:46Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<pre><code>git reset --soft HEAD~1</code></pre><p>Soft: This will remove the commit from local only and keep the changes done in files as it is.</p><pre><code>    git reset --hard HEAD~1    git push origin master</code></pre><p>Hard: This will remove that commit from local and remote and remove changes done in files.</p>    ",
      "comments": []
    },
    {
      "rating": "4",
      "date": "",
      "edited": "2021-01-18 20:26:51Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>If the repository has been committed locally and not yet pushed to the server, another crude/layman way of solving it would be:</p><ol><li>Git clone the repository in another location.</li><li>Copy the modifications (files/directories) from the original repository into this new one. Then commit and push with the changes from the new one.</li><li>Replace the old repository with this new one.</li></ol>    ",
      "comments": []
    },
    {
      "rating": "4",
      "date": "",
      "edited": "2021-01-18 20:33:24Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><code>git reset HEAD@{n}</code> will reset your last <em>n</em> actions.</p><p>For reset, for the last action, use <code>git reset HEAD@{1}</code>.</p>    ",
      "comments": []
    },
    {
      "rating": "4",
      "date": "2022-08-10 10:48:50Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>If you have made local commits that you don't like, and they have not been pushed yet you can reset things back to a previous good commit. It will be as if the bad commits never happened. Here's how:</strong></p><p>In your terminal (Terminal, Git Bash, or Windows Command Prompt), navigate to the folder for your Git repo.Run git status and make sure you have a clean working tree.Each commit has a unique hash (which looks something like 2f5451f). You need to find the hash for the last good commit (the one you want to revert back to). Here are two places you can see the hash for commits:In the commit history on GitHub or Bitbucket or website.In your terminal (Terminal, Git Bash, or Windows Command Prompt) run the command git log --onlineOnce you know the hash for the last good commit (the one you want to revert back to), run the following command (replacing 2f5451f with your commit's hash):</p><pre><code>git reset 2f5451fgit reset --hard 2f5451f</code></pre><p>NOTE: <em>If you do git reset the commits will be removed, but the changes will appear as uncommitted, giving you access to the code. This is the safest option because maybe you wanted some of that code and you can now make changes and new commits that are good. Often though you'll want to undo the commits and throw away the code, which is what git reset --hard does.</em></p>    ",
      "comments": []
    },
    {
      "rating": "3",
      "date": "",
      "edited": "2021-06-23 22:30:20Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>Firstly, run this command to reset the <em>commit</em>:</p><pre><code>git reset --hard HEAD~1</code></pre><p>Secondly, run this command to push the new <em>commit</em>:</p><pre><code>git push upstream head -f</code></pre>    ",
      "comments": []
    },
    {
      "rating": "3",
      "date": "2022-10-20 05:43:14Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can use the &quot;git reset&quot; command to remove the commits from your local repository.</p><p>git reset --hard HEAD~</p><p>This will remove the most recent commit from your repository. If you have made multiple commits, you can remove them all by substituting the number of commits for &quot;~&quot;. For example, to remove the last two commits, you would use:</p>    ",
      "comments": []
    },
    {
      "rating": "2",
      "date": "",
      "edited": "2021-06-23 22:27:58Z",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>You can use the <code>git reset unwanted_file</code> command with the file you don't want to stage. By using this command, we can move the changed file from the staging area to the working directory.</p><p><em><a href=\"https://codeburst.io/how-to-undo-commits-in-git-locally-remotely-10078152c239\" rel=\"nofollow noreferrer\">How can I undo commits in Git locally and remotely?</a></em></p>    ",
      "comments": []
    },
    {
      "rating": "2",
      "date": "2021-11-25 11:15:48Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p><strong>IN CASE IF YOU WANT TO REVET TO A LAST COMMIT AND REMOVE THE LOG HISTORY AS WELL</strong></p><p>Use below commandlets say you want to go to previous commit which has commitID SHA - <strong>71e2e57458bde883a37b332035f784c6653ec509</strong>the you can point to this commit it will not display any log message after this commit and all history will be erased after that.</p><p><strong>git push origin +71e2e57458bde883a37b332035f784c6653ec509^:master</strong></p>    ",
      "comments": []
    },
    {
      "rating": "0",
      "date": "2022-10-12 08:34:08Z",
      "edited": "",
      "author": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "editor": {
        "name": "",
        "reputation": "",
        "gbadge": "",
        "sbadge": "",
        "bbadge": ""
      },
      "content": "<p>In Visual Studio, simply revert your commit then push it together with the auto created revert commit. Everything is shown in the screen shots below</p><p><a href=\"https://i.stack.imgur.com/ZS89l.png\" rel=\"nofollow noreferrer\"><img src=\"https://i.stack.imgur.com/ZS89l.png\" alt=\"enter image description here\" /></a></p><p><a href=\"https://i.stack.imgur.com/JwpB3.png\" rel=\"nofollow noreferrer\"><img src=\"https://i.stack.imgur.com/JwpB3.png\" alt=\"enter image description here\" /></a></p><p><a href=\"https://i.stack.imgur.com/wa0aJ.png\" rel=\"nofollow noreferrer\"><img src=\"https://i.stack.imgur.com/wa0aJ.png\" alt=\"enter image description here\" /></a></p><p><a href=\"https://i.stack.imgur.com/4yZtF.png\" rel=\"nofollow noreferrer\"><img src=\"https://i.stack.imgur.com/4yZtF.png\" alt=\"enter image description here\" /></a></p><p><a href=\"https://i.stack.imgur.com/HcSJi.png\" rel=\"nofollow noreferrer\"><img src=\"https://i.stack.imgur.com/HcSJi.png\" alt=\"enter image description here\" /></a></p>    ",
      "comments": []
    }
  ]
}
```

## Usage

    stackexchange directory [URLS...]

All options should be specified before the directory.

The script writes every question to file in the directory (without overwriting them) and names of the files is the ids of questions.

Size of the page is 50 questions.

Since ids of the questions overlap with different websites it is recommended to download them into separate directories.

Download all tags to directory x with delay of 0.2 second and randomness of 3 seconds

    stackexchange -d 0.2 -r 3 x 'https://stackoverflow.com/tags'

Download all questions tagged python to directory x while using 5 processes

    stackexchange -p 5 x 'https://stackoverflow.com/questions/tagged/python'

Download all questions starting from 50th page to 75th page to directory x

    stackexchange -f 50 -l 75 x 'https://stackoverflow.com'

Download some questions to directory x

    stackexchange x 'https://stackoverflow.com/questions/3737139/reference-what-does-this-symbol-mean-in-php' 'https://stackoverflow.com/questions/60174/how-can-i-prevent-sql-injection-in-php' 'https://stackoverflow.com/questions/391005/how-can-i-add-html-and-css-into-pdf'

Get some help

    stackexchange -h

## Issues

### Too Many Requests

Be warned that you should limit your requests to all of the websites from stackexchange family as the protection counts requests for all of them.

### Large directory feature is not enabled on this filesystem

Storing a lot of files in a single directory (around 7 milion) will cause bash to show errors of lack of space (even when there is space and free inodes).

Dmesg will show you something like this:

    [979260.318526] EXT4-fs warning (device sda1): ext4_dx_add_entry:2516: Directory (ino: 89391105) index full, reach max htree level :2
    [979260.318528] EXT4-fs warning (device sda1): ext4_dx_add_entry:2520: Large directory feature is not enabled on this filesystem