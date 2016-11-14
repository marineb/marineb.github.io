---
layout: post
title:  "Deploy Static Sites With Git to Servers Like Dreamhost"
date:   2016-11-13 22:33:20 -0500
---

I originally researched this a while back but couldn't figure it out then. Until today, I've been using [Deploybot](https://deploybot.com/) which does the job (deploying websites) for you: pretty great product. They recently got rid of their free service and I didn't want to pay for a task that would *seem* so simple to setup.

```
TODO: Ideally, I'd like to make this work via Github's webhooks (any update to a specific branch would triger a deploy). I want to figure that part out at some point. 
```

## On your server
I'm on both Amazon and Dreamhost servers. But in this case, I wanted to set this up on Dreamhost. 

First you'll need to ssh into your server: 

```
$ ssh [username]@[yourdomain.com] 
```

Be sure to change `yourdomain.com` with the actal domain or server you want to connect to. Same for `username` obviously. 

You will be asked to enter your password (though here's is how to [configure passwordless login](https://help.dreamhost.com/hc/en-us/articles/216499537-How-to-configure-passwordless-login-in-Mac-OS-X-and-Linux)). 

Next we're going to setup the remote Git repository. 

```
$ mkdir yourwebsite.com.git
$ cd yourwebsite.com.git
$ git init --bare
```

Now we're going to create a Git hook so that when we push our site to this remote repository, it will get copied to your site directory.

```
$ cd ..
$ ls
```

You should probably see in there both yourwebsite.com.git and yourwebsite.com. If you don't see yourwebsite.com, add it. 

```
$ mkdir yourwebsite.com
```

Next, we'll go inside `yourwebsite.com.git` and see what the path is. 

```
$ cd yourwebsite.com
$ pwd
```

Take note of that path, it will become handy later.

Now we're going to create a post receive hook so that when you'll be pushing your site to the git repo, it will copy its content to `yourwebsite.com`. 

```
$ vi hooks/post-receive
```

This will open this `post-receive` file, in edit/insert mode. Paste the following in it:

```
$ git --work-tree=/home/yourusername/yourwebsite.com --git-dir=/home/yourusername/yourwebsite.com.git checkout -f
```

Make sure the paths match the path I told you to save earlier. You want to get this part right: `/home/yourusername/`. 

Now, let's save the file: do escape, and `wq` and enter. `wq` stands for `write` and `quit`. If that's new to you, you might want to familiarize yourself with the vi editor, and unix commands. 

Now run `chmod +x hooks/post-receive` to insure that this newly create file, this hook, is executable (that's the `x`). More info on chmod [here](https://en.wikipedia.org/wiki/Chmod). 

Ok. We're done with the server setup. Type `exit` to close your ssh connection. 

## Setting up the push command

Go inside your local repository. 
You're probably already already using something like `git push origin master` to push your code up (to github or elsewhere). 

Now we're going to setup a command that will allow you to push your code to that git repo you created on the server. 

```
$ git remote add web ssh://yourusername@yourdomain.com/home/yourusername/yourwebsite.com.git
$ git push web +master:refs/heads/master
```

That's it, your code is pushed to your new remote `web`. 

Here we forced pushed your repo to master to make sure the changes would push through. But moving forward, just do `git push web master`. 


## Creating a new remote "all"
Now if you're like me, you may not enjoy doing 2 git pushes each time. So let's setup a remote that will push to your github repo, and to your server repo. 

```
$ git remote -v
```

This will show you all the remotes you've already setup. Now let's create the `all` remote. 

```
$ git remote add all git://theRepoWhichIsSetupOnTheOriginRemote.git
$ git remote set-url --add --push all ssh://yourusername@yourdomain.com/home/yourusername/yourwebsite.com.git
$ git remote set-url --add --push all  $ git://theRepoWhichIsSetupOnTheOriginRemote.git
```

Now running `git push all master` will push your master branch to both your github repo and to your server repo. 

That's it. 

If you have any feedback, you can find me [@marineboudeau](http://twitter.com/marineboudeau). 


**References**

* [https://brandonevans.ca/post/text/deploying-websites-with-git-on-dreamhost/](https://brandonevans.ca/post/text/deploying-websites-with-git-on-dreamhost/)
* [http://toroid.org/git-website-howto](http://toroid.org/git-website-howto)
* [http://stackoverflow.com/questions/14290113/git-pushing-code-to-two-remotes](http://stackoverflow.com/questions/14290113/git-pushing-code-to-two-remotes)
