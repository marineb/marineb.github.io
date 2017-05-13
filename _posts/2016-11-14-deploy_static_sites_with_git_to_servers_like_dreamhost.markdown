---
layout: post
title:  "Deploy Static Sites With Git to Servers Like Dreamhost"
date:   2016-11-13 22:33:20 -0500
---

---


h1. A clean version of this is available [here](https://medium.com/@marineboudeau/deploy-static-sites-with-git-to-servers-like-dreamhost-8d15e2b6bc41)


How I setup one command to deploy my site to Dreamhost and push its code to Github


A few very basic unix command before we get started (most of you will skip this)
If you’re not familiar with unix command, these will be useful when we’re on the server so you can navigate properly.
ls = lists the files and directories contained in the directory you’re currently in
pwd = the path you’re currently in
cd yourdirectory = to go inside a directory
cd .. = to go back in the directory above
On your server
Create the remote repo
First you’ll need to ssh into your Dreamhost server:
$ ssh [username]@[yourdomain.com]
Be sure to change yourdomain.com with the actual domain or server you want to connect to. Same for username obviously.
You will be asked to enter your password (though here’s how to configure passwordless login).
If you don’t see yourwebsite.com in your home directory, add it:
$ mkdir yourwebsite.com
Next we’re going to setup the remote Git repository:
$ mkdir yourwebsite.com.git
$ cd yourwebsite.com.git
$ git init --bare
You should be getting a response like this:
Initialized empty Git repository in /home/yourusername/yourdomain.com.git/
Keep a mental note (or written note) of that file path. We’ll need it later.
Create a post receive hook
Now we’re going to create a post receive hook: when you’ll be deploying your site to the git repo, it will copy its content to yourwebsite.com.
$ vim hooks/post-receive
This will open thepost-receive file, in edit/insert mode.
Paste the following in it:
git --work-tree=/home/yourusername/yourwebsite.com --git-dir=/home/yourusername/yourwebsite.com.git checkout -f
Make sure the paths match the path we saved earlier. You especially want to get this part right: /home/yourusername/.
Now, let’s save the file: do escape, and wq and enter.
Note: wq stands for write and quit. If that’s new to you, you might want to familiarize yourself with terminal text editors (vim, vi, etc).
Next, we’ll run a command to insure that this newly create file, this hook, is executable. More info on chmod here.
chmod +x hooks/post-receive
Ok. We’re done with the server setup. Type exit to close your ssh connection.
Setting up the push command
Go inside your local repository. You’re probably already already using something like git push origin master to push your code up (to github or elsewhere) to the master branch for instance.
So instead of creating a new remote, we’re going to redefine the remote “origin” so that it will push not only to github, but also to your server.
Recreate the remote “origin”
$ git remote set-url --add --push origin ssh://yourusername@yourdomain.com/home/yourusername/yourwebsite.com.git
# You're adding the server as the first place to push your code to
$ git remote set-url --add --push origin git@github.com:repo-org-or-user/repo-name.git
# You're adding your github repo as the second place to push your code to
$ git push origin +master:refs/heads/master
This last command will force push to your repos. Only do that once. Moving forward, just use git push origin master. This will push your master branch to both your github repo and to your server repo.
And of course, git push origin mynewbranch to push to mynewbranch.
That’s it.
Feel free to ❤️ this post or share it if it was useful. You can also get in touch at marineboudeau.com or on Twitter @marineboudeau.


---

References
brandonevans.ca/post/text/deploying-websites-with-git-on-dreamhost/
toroid.org/git-website-howto
stackoverflow.com/questions/14290113/git-pushing-code-to-two-remotes
