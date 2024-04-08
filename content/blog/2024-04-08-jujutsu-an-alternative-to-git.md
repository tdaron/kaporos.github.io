+++
title="Jujutsu: An alternative to git."
date=2024-04-08
authors=["Théo Daron"]
+++

# What is Jujutsu

[Jujutsu](https://martinvonz.github.io/jj/latest) is a Version Control System (VCS), just like Git, which means it helps developers work together on projects while keeping track of changes over time. It's super handy, almost a must-have in today's development world.

Now, Git has been around for a while. I mean, it first popped up back in 2005, so that's like two decades ago! But despite its age, Git is still the king of VCS. Why? Well, because it's darn good at what it does, and it's become a staple in developers' workflows.

Jujutsu tries to solve problems Git has without reinventing the wheel as Jujutsu borrows a lot of concepts of Git, and ATM, as Jujutsu's native back-end is not finished yet, so they're using git as storage solution, making it **compatible with existing git repositories.**  

(I'm currently using it to manage this blog, which happens to be hosted on Github)

# Jujutsu Concepts
I will use the word 'commit' for convenience, but in Jujutsu, a commit is referred to as a revision.

## Branches

There are some differences between Git and Jujutsu. One of the most significant ones, I believe, is that Jujutsu doesn't incorporate the concept of a "current branch" (though it does support branches).

> A branch is just a pointer to a commit.

In Git, when you make a commit, the branch you're currently on automatically updates to point to the new commit.

However, with Jujutsu, making a commit doesn't affect any branches. Instead, you're always on what you could call an "anonymous branch," which essentially represents your current commit.

For instance, take a look at this output from jj log (the Jujutsu equivalent of git log):

```
@  mwpvwptr theo@daron.be 2024-04-08 15:03:28 97d19080
│  some great revision
◉  pzxyqynk theo@daron.be 2024-04-08 15:02:59 cool-branch 4e950f42
│  another edit of toto file
│ ◉  pzwlrmqw theo@daron.be 2024-04-08 15:02:38 42c5ce44
├─╯  editing toto file
◉  rzzuolln theo@daron.be 2024-04-08 15:02:17 4fd2d439
│  adding toto file
◉  zzzzzzzz root() 00000000

```

You can see that I have a branch named cool-branch pointing to my "another edit of toto file" revision.

However, for example, my "editing toto file" revision, which is in another branch, doesn't have a branch name; it's what we call an "anonymous branch."

And my current working revision is the latest one, denoted by an "@" symbol in front of it. As you can see, no branch currently points to it. But I could move my cool-branch to point to it using the command jj branch set cool-branch.

## No working directory

As you probably know, in Git, there's something called the working directory, which essentially consists of any uncommitted changes inside your repository.

However, in Jujutsu, you don't have these "uncommitted changes." Instead, all your changes are immediately added to your current revision. So, let's say I'm currently on "some great revision," and I make a change; that change gets automatically included in that same revision. This means that if we want to rename the revision after making more changes, we can. And if we want to name it before making changes to remember what we were doing, that's possible too.

This has some interesting implications. For instance, there's no longer a need for the "stash" functionality. If you want to check out another revision, you can just do it. There's no requirement to make a dirty commit or stash to save the current working directory before switching to another revision, as changes you made already are saved in a revision.

Moreover, it also means you can edit revisions in the past. In Jujutsu, you're allowed to check out a previous revision, make some changes, and it will automatically rebase all descendant commits on this edited revision.

## Conflicts resolutions

Since Jujutsu doesn't have a working directory concept, conflicts are handled differently. A revision can find itself in conflict with some parent revisions (for instance, when merging a branch). This conflict state gets stored within the revision itself, but you won't find yourself stuck with merge conflicts in your current working directory like you might in Git.

So, if a conflict arises, no big deal. We can simply switch gears and work on something else, knowing that we can tackle the conflicts later on.

Now, if we decide to edit a past revision to resolve a conflict, this resolution automatically cascades down to all descendant commits, thanks to the magic of automatic rebasing when editing past revisions. Trust me, it makes life wonderful haha. 

# So..should I switch to Jujutsu ?

It wouldn't be fair to say that Jujutsu is superior to Git and that Git is terrible. Jujutsu aims to address some of Git's shortcomings while still retaining its fundamental concepts like commits (or revisions) and branches. Plus, it's compatible with Git repositories, which is pretty cool.

In my experience, using Jujutsu is more enjoyable because it allows you to navigate your revision tree freely. It's easier to edit past revisions and resolve conflicts since Jujutsu won't leave you stuck in a state where you're forced to fix something before you can continue working, unlike Git.

I definitely recommend checking out Jujutsu's website and GitHub repository. You might find it to be a valuable addition to your workflow. Here are the links: [website](https://martinvonz.github.io/jj/latest) and [GitHub](https://github.com/martinvonz/jj).
