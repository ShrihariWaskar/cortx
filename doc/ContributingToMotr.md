## Code reviews and commits

### Git setup on development box
Update Git to the latest version
with older git version, you might be getting errors with commit hook, like this one:


> $ git commit
> git: 'interpret-trailers' is not a git command. See 'git --help'.
cannot insert change-id line in .git/COMMIT_EDITMSG

Fix (for CentOS 7.x):

 > $ yum remove git
 
 > $ yum -y install  https://centos7.iuscommunity.org/ius-release.rpm
 
 > $ yum -y install  git2u-all
 
 > $ yum-config-manager --disable ius/x86_64 # prevent accidental updates from this repo
 
Setup the git config options

 > $ git config --global user.name ‘Your Name’
 
 > $ git config --global user.email ‘Your.Name@seagate.com’

### To work on a feature and submit review to GitHub

Clone cortx-motr

    Each contributor needs to do 'fork' to create their own private cortx-motr repository.
        Go to homepage of 'cortx-motr' repository on GitHub, there you will see 'fork' at top right corner.

        $ git clone git@github.com:"your-github-id"/cortx-motr.git


Ensure you have checkout “main” branch

> $ git checkout main

Now checkout your new branch for saving your code
Example git checkout -b <username>/<feature>
Username = name or initials, example “John” or just “JB”
> $ git checkout -b 'your-local-branch-name'
> $ git checkout -b JB/motr_sync

Make code changes

Add files to be pushed to git to staged area

> $ git add foo/somefile.cc

Add all such files

Make sure build passes locally using [these steps](#compilation-and-running-unit-test) & commit your changes

> $ git commit -m "GID - Appropriate Feature/Change Description"

Check git log to see your commit, verify the author name

> $ git log 

If author name is not set properly then set using following command

> git commit --amend --author="Author Name < email@address.com >"

 * Make sure your [git configs options](#getting-git--gerit-to-work) are set properly.

Note here the commit hook should add a ChangeID, something like 
~~~
commit a41a6d839148026cc0c3b838352529e10898e5dc
Author: Rajesh Nambiar <rajesh.nambiar@seagate.com>
Date:   Thu Apr 16 00:55:01 2020 -0600

    EOS-7148: This parameter not supported by systemd in our hardware

    Change-Id: I1ce3d04e74d56c11645a95b1d523e72b0cc01e17
~~~

Push your changes to GitHub

    $ git push origin 'your-local-branch-name'
    
* Before pushing, TAKE A PAUSE. Have you [rebased your branch](#How-to-rebase)? if not then doing right now to avoide merge conflicts on github

Open pull request for review

    Open the URL given in the output of 'git push' command above.

    Select base:branch as 'main' from the dropdown.

    click 'Create pull request' to create the pull request.

    Add reviewers to get feedback on your changes.

### How to rebase your local branch on latest master?

    $ git checkout master

    $ git pull origin master

    $ git submodule update --init --recursive

    $ git checkout 'your-local-branch'

    $ git pull origin 'your-remote-branch-name'

    $ git submodule update --init --recursive

    $ git rebase origin/master

    If you get conflicts, follow the steps mentioned in the error message from git.

## Running Jenkins / System tests

* To get familiar with jenkins please visit [here](https://en.wikipedia.org/wiki/Jenkins_(software)).

### How to start Motr jenkins job?
(Note: Change this part from gerrit to github.Screenshot shows gerrit information)
1. Open Motr jenkins [link](http://eos-jenkins.colo.seagate.com/job/Release_Engineering/job/InnerSource/job/EOS-Core/job/mero-vm-test/). (Note: Edit this link)
2. Login with Username as `eos-core` and password as `eos-core`.
3. Click on `Build With Parameters` option.
4. You need to enter the `GIT_REFSPEC` and `label`, follow the steps as mentioned below.
  * To get `GIT_REFSPEC` goto your gerrit server branch where you pushed your changes and click on`Download` section. The highlighted part `refs/changes/31/19331/4` in `checkout` section as shown below is your `GIT_REFSPEC`. Copy and paste it on `GIT_REFSPEC` section of jenkins.
  
  
<p align="center"><img src="../../assets/images/mero_gerritsnap_jenkins_GIT_REFSPEC_detail.JPG?raw=true"></p>

  * `label` field could be anything which is easy to recognize. It isn't mandatory field though.
5. Press on `Build` button to start building the code. 
6. All the currently running builds can be monitored from `Build History` section on left side pane. Specific build can be monitored by clicking on build number/lable from `Build History` section.

### Wanna test specific Motr commit with s3?

* If s3server setup is not done yet then kindly visit [s3serverQuickstart](CortxS3ServerQuickStart.md) and get your setup ready.
* If s3 is already setup then check out detailed steps [here](CortxS3ServerQuickStart.md#Testing-specific-MERO-version-with-S3Server).

### You're all set & You're awesome

In case of any query feel free to write to our [SUPPORT](SUPPORT.md).

Let's start without a delay to contribute to seagate's open source initiative and join this movement with us keeping a common goal of making data storage better, more efficient and more accessible.

Seagate welcomes You! :relaxed: