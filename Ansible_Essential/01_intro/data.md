# Environment Setup
---

It's take some time to review the environment typology that we're going to use throughout this course

that apology we're using is a common three tier web application architecture.

We have a load balancer tear along with the application tier and databased here.

In addition in addition to these three commentaries we have a control machine that sits off to the side

and the control machine really acts as the orchestration of the place that we're going to install and

spool and do all of our mission management throughout this course.

I encourage you to follow along by having your own environment that you can use throughout lectures

.

You can do this in many ways.

If you have a bare metal environment nonprofit or given varmint that you have access to that's great

.

You can also use the cloud environment using a hosting provider of your choice to set up CMS that that

mirror this typology.

There are also some good local options that you could run on your own workstation.

One of those is a vagrant which is a wrapper around a virtual box and be more fusion and allows you

to spin up dev environments very very easily.

And the last option the one that I'll be using throughout this course is a Docker environment using

the docker tool box with Docker machine and Dr. compose.

I'll be making a few assumptions about the environment that are true of my lab environment here my environment

.

And if you're going to follow along it would be good to check these assumptions against your own environment

to minimize any conflicts you might find against lectures that I'll be doing.

The first assumption is that you have access to a control machine recone install installations.

There are a few caveats around the control machine.

And if I pull up a box that ansible dot com you see those listed here.

So if you're on the installation page there are controlled machine departments you can see that ansible


But there's a heavy caveat that you cannot use Windows as the control machine.

So ansible supports Windows as a target even though we won't be covering Windows configuration in this

lecture.

If you have Windows servers you can use Aspel to manage those but it's not supported as a control machine

.

Many distributions of Linux are supported So if you have a Windows workstation or server as your base

machine you can spin up a VM or you can also just create a VM in your environment that's running Linux


The second assumption I'm making about the full environment is that all of the end servers are going

to be running a good trustee and that is a very common Linux distribution.

Obviously it's not a requirement for you to use that in your real world environments.

But everything we're doing here and throughout this course will use a good trustee as the base OS.

Third assumption is that all commands are going to be run from the control machine.

So throughout the lecture you'll see me executing ansible and Ansdell playbook commands.

Just know that unless I say otherwise I will be running those from the control sheet and not from my

local laptop.

In your case if you have a Mac or a Linux workstation and you choose to use that as your control machine

they may be.

Those commands will execute from your laptop.

But everything that I'm running will be running from the role of the control machine in my environment

.

Fourth assumption is that the control machine has S-sh access already set up so all the other hosts

.

There is a lecturer in the appendix about S-sh public key trusts and how to set those up.

If you're not familiar with that ansible does support using normal username password authentication

but for ease of use and just quicker access into the host site.

I've gone ahead and set up S-sh access from my control machine to my other hosts.

So if you have an environment that you want to set up I'd recommend going ahead and getting that taken

care of out of the way before we start to lectures.

And the last assumption I'm making is that the user that you set up to use to have X ansible run as

will have super user access on your posts.

So this is not a strict requirement to actually run ansible and still can run without super user access

.

But since we're doing configuration management systems configuration it's almost a given that at some

point you're going to need do something that requires sudo or root or super user access.

In my case I've set up a pseudo host file and create an answer with user who's in the suitor's group

and can execute commands without a password.

Just for ease of use whatever your local policy is or preference on the running users add super user

in your environment you can set that up.

I'll discuss ways that you can elevate privilege through the lectures but know that the answer please

is that I'm using.

You've seen me use is already set up for you to access.

Now for all this I do encourage you to set up an environment because I think that's the best way for

you to learn it and try out the commands and test them out if your environment differs a little bit

.

That's fine.

Just keep in mind that those differences you'll have to track as we go through the course.

As you watch me execute those commands in these videos.

So if you're interested in following along I'd recommend taking some time now to go ahead and set up

an environment that matches and will jump into installation and configuration that

---

# Installation
 ---

The first step on our journey to mastering simple is to get ansible installed so that we can execute

it.

I pulled up here Dr. ansible dot com and gone to the installation page.

There are a variety of ways that you can get ansible installed.

And suppose I saw an application so it's available via app which is basically universal installed regardless

of which distribution you're using.

If you're using a Linux host has your control machine which we are in our environment here there are

packages available through package repositories.

And I would recommend using package repository if it's available for your distribution.

You're not using a boon to your environment.

You can see that there's some other options here you portage package pending on which which distribution

you're using.

I'm going to jump down to the intersection here and install via your I have an S-sh session to my control

machine which has a fresh going to install the of changes I've made so far are just setting up the S-sh

relationships and the users that I can log in to my posts.

So let's get started with the installation.

We're going to build is available via a PPA.

And this is managed by the company behind ansible.

So that means we'll get a nice up to date version of ansible that tracks with the latest development

.

First step in using a PPA is to install software properties common

and this gives us access to the app data repository command.

We can then use to configure VPN in older versions of a boon to instead of software properties common

you'd use Python suffer properties.

You can see in this note in the documentation

now that that's installed we have access to have had Depository and well by the handsful PPA

.

OK.

Next step is to run an update which queries our new repository or your PPA and pulls down all the metadata

for our Ansible packages.

And at this point we can do to to install ansible

Now that ansible install those checks that we have access to the ansible command line tool.

And we do also need to use ansible playbook and the ansible Galaxie tool.

Now that anthem was installed we'll spend some time laying a foundation for connecting and managing

those before we go on to executing tasks and writing playbooks.
