---
title: Collaborating
teaching: 25
exercises: 0
questions:
- "How can I use version control to collaborate with other people?"
objectives:
- "Clone a remote repository."
- "Collaborate by pushing to a common repository."
- "Describe the basic collaborative workflow."
keypoints:
- "`git clone` copies a remote repository to create a local repository with a remote called `origin` automatically set up."
---

For the next step, we will work in a group to all share the same repository. One person will be the "Owner" and the others
will be "Collaborators". The goal is that the Collaborators add changes into
the Owner's repository. 

> ## Practicing By Yourself
>
> If you're working through this lesson on your own, you can carry on by opening
> a second terminal window.
> This window will represent your partner, working on another computer. You
> won't need to give anyone access on GitHub, because both 'partners' are you.
{: .callout}

The Owner needs to give the Collaborator access. On GitHub, click the settings
button on the right, select Manage access, click Invite a collaborator, and
then enter your partner's username.

![Adding Collaborators on GitHub](../fig/github-add-collaborators.png)

To accept access to the Owner's repo, the Collaborator
needs to go to [https://github.com/notifications](https://github.com/notifications).
Once there she can accept access to the Owner's repo.

Next, the Collaborator needs to download a copy of the Owner's repository to her
machine. This is called "cloning a repo".

We'll be using a repository listing the locations of people's favourite pubs, restaurants, cafes and just general favourite places to go. You can find this repository at [https://github.com/NOC-OI/favourite-places](https://github.com/NOC-OI/favourite-places)

To clone the Owner's repo into
her `Desktop` folder, the Collaborator enters:

~~~
$ git clone git@github.com:NOC-OI/favourite-places.git ~/Desktop/favourite-places
~~~
{: .language-bash}


If you choose to clone without the clone path
(`~/Desktop/favourite-places`) specified at the end,
you will clone inside your own favourite-places folder!
Make sure to navigate to the `Desktop` folder first.

![After Creating Clone of Repository](../fig/github-collaboration.svg)

The Collaborator can now make a change in her clone of the Owner's repository,
exactly the same way as we've been doing before:

~~~
$ cd ~/Desktop/favourite-places
$ nano places.csv
$ cat places.csv
~~~
{: .language-bash}

~~~
name,symbol,creator,comments,lon,lat
Express Cafe,cafe,Vlad,black pudding,-4.081978,52.414381
~~~
{: .output}

~~~
$ git add places.csv
$ git commit -m "Adding Express Cafe"
~~~
{: .language-bash}

~~~
 1 file changed, 1 insertion(+)
 create mode 100644 places.csv
~~~
{: .output}

Then push the change to the *Owner's repository* on GitHub:

~~~
$ git push origin main
~~~
{: .language-bash}

~~~
Enumerating objects: 4, done.
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 306 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:NOC-OI/favourite-places.git
   9272da5..29aba7c  main -> main
~~~
{: .output}

Note that we didn't have to create a remote called `origin`: Git uses this
name by default when we clone a repository.  (This is why `origin` was a
sensible choice earlier when we were setting up remotes by hand.)

Take a look at the Owner’s repository on GitHub again, and you should be 
able to see the new commit made by the Collaborator. You may need to refresh
your browser to see the new commit.

> ## Some more about remotes
>
> In this episode and the previous one, our local repository has had
> a single "remote", called `origin`. A remote is a copy of the repository
> that is hosted somewhere else, that we can push to and pull from, and 
> there's no reason that you have to work with only one. For example, 
> on some large projects you might have your own copy in your own GitHub
> account (you'd probably call this `origin`) and also the main "upstream"
> project repository (let's call this `upstream` for the sake of examples).
> You would pull from `upstream` from time to 
> time to get the latest updates that other people have committed.
>
> Remember that the name you give to a remote only exists locally. It's
> an alias that you choose - whether `origin`, or `upstream`, or `fred` -
> and not something intrinstic to the remote repository.
>
> The `git remote` family of commands is used to set up and alter the remotes
> associated with a repository. Here are some of the most useful ones:
>
> * `git remote -v` lists all the remotes that are configured (we already used
> this in the last episode)
> * `git remote add [name] [url]` is used to add a new remote
> * `git remote remove [name]` removes a remote. Note that it doesn't affect the 
> remote repository at all - it just removes the link to it from the local repo.
> * `git remote set-url [name] [newurl]` changes the URL that is associated 
> with the remote. This is useful if it has moved, e.g. to a different GitHub
> account, or from GitHub to a different hosting service. Or, if we made a typo when
> adding it!
> * `git remote rename [oldname] [newname]` changes the local alias by which a remote 
> is known - its name. For example, one could use this to change `upstream` to `fred`.
{: .callout}

To download the Collaborator's changes from GitHub, the Owner now enters:

~~~
$ git pull origin main
~~~
{: .language-bash}

~~~
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From git@github.com:NOC-OI/favourite-places.git
 * branch            main     -> FETCH_HEAD
   9272da5..29aba7c  main     -> origin/main
Updating 9272da5..29aba7c
Fast-forward
 places.csv | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 places.csv
~~~
{: .output}

Now the three repositories (Owner's local, Collaborator's local, and Owner's on
GitHub) are back in sync.

> ## A Basic Collaborative Workflow
>
> In practice, it is good to be sure that you have an updated version of the
> repository you are collaborating on, so you should `git pull` before making
> our changes. The basic collaborative workflow would be:
>
> * update your local repo with `git pull origin main`,
> * make your changes and stage them with `git add`,
> * commit your changes with `git commit -m`, and
> * upload the changes to GitHub with `git push origin main`
>
> It is better to make many commits with smaller changes rather than
> of one commit with massive changes: small commits are easier to
> read and review.
{: .callout}

> ## Group exercise
> 1. Everyone in group should clone the repository git@github.com:NOC-OI/favourite-places.git (using SSH), or if you are unable to use SSH then clone https://github.com/NOC-OI/favourite-places.git
> 2. Find the longditude and latitude of your favourite pub, cafe, restaurant, etc. The website https://www.latlong.net/ or https://www.gps-coordinates.net/ can help you do this. 
> 3. Add a new line to places.csv with the following fields seprated by commas: The name of the location, a symbol (bar, cafe, restaurant), your name, a comment about the location, the longitude (note this should be negative for the Western Hemisphere, including most of the UK) and latitude. 
> 4. Add/Commit this change to your local repository.
> 5. Push your changes to the upstream repository.
> 6. Unless you are the first person to push to the repository, this will almost certainly result in a merge conflict error since multiple people have edited the same line. We'll deal with how to resolve this in the next section.
{: .challenge}

> ## Review Changes
>
> The Owner pushed commits to the repository without giving any information
> to the Collaborator(s). How can the Collaborator(s) find out what has changed with
> command line? And on GitHub?
>
> > ## Solution
> > On the command line, the Collaborator(s) can use ```git fetch origin main```
> > to get the remote changes into the local repository, but without merging
> > them. Then by running ```git diff main origin/main``` the Collaborator
> > will see the changes output in the terminal.
> >
> > On GitHub, the Collaborator(s) can go to the repository and click on 
> > "commits" to view the most recent commits pushed to the repository.
> {: .solution}
{: .challenge}

{: .challenge}
