Right with ansible and stalls we're ready to start executing our first commands now with ansible every

command has both the content of the change you want to make or the command that you want to run.

But also the target that you want to run against with the ansible tool you have to specify all of your

potential targets in advance.

And the way that you do that is through populating an inventory now inventory can take a few forms.

One is it's a simple static file that has a list of hosts.

The second is using a dynamic inventory which is normally implemented as a script that Ben calls some

other API or service to populate all of the host information and variable data.

In our case where are you going to focus on the static inventory first and just populate a simple list

of hosts for our environment.

Since our environment just has a few of those to deal with so the inventory lists the hosts but it can

also list additional information alongside those hosts names.

And mostly those extra details are around how you connect to the host.

So if you're using a non-standard S-sh port or a special hostname to connect to the host you can specify

that in the inventory and so will also give you the ability to specify that on the command line per

invocation.

But by putting it into the inventory you can trust that whenever you connect to that host the right

parameters for that host will be used.

The Natori also allows you to group posts together by role and you can then use those groups as part

of your target's one and executing commands and ansible and you can also provide a variable values and

past variable data in the inventory.

Like I said in our case we're going to stick to a very simple static inventory with just lists of host

Group Birol by default and the pull gives us a sample inventory that's populated when the tool is first

installed and spel also gives us a handy tool that we can use to query the inventory and see what hosts

are populated.

So here on the control host fight type ansible lists hosts and provide a target pattern.

In this case I'll use all just a short cut for every host that's in the inventory.

You'll see that a lot of dummy hosts are populated here that we would expect to reach in our environment

.

But this there is a template for you to use in filling out your inventory file so that file is located

by default at this location.

That's the ansible posts.

I will go in here and delete out all of these so we don't have a conflict later on with our actual list

.

Now if I run Ansible host again we see that no close match is a good default sane state to be in.

Now adding our own hosts we could put them back into this file at the ansible house and then continue

working with ansible.

The problem with that is that we would then have to edit this file that's on the control hosts in the

system location every time we want to make a change to the inventory and we can't keep those posts in

a file located in our local directory and get repository.

So part of managing our infrastructure is code really is to use a process that allows us to commit every

change into a repository and track that changes to that inventory along with the answer will playbooks

and the rolls and all of the changes we want to make to our infrastructure.

So rather than editing the system default at the ansible hosts My recommendation is to create a local

inventory file and populate that amatory file with your hosts and keep that next to your playbooks and

your roles inside of your get repo.

And that way you can track the changes over time.

So up next we'll create our own local inventory file and populate our hosts in that final

So let's take a look at creating our own inventory file that has our hosts from our scenario populated

in it.

So here in our local directory create an ansible folder and inside of this ansible folder I'm going

to create a file that I'm going to called Dev and populate the hosts.

Now the format for the inventory file can be just a simple list of names in which case those hosts would

just be connected to directly and reached by hostname.

You can also group posts together into arbitrary groups.

In our case we're going to use the host's role or function and in our scenario as their group name.

So we'll create a group for the load bouncer's which will have the bouncer LBO one will have a group

for the web servers and a group for the databases and also group for the control host.

So in our file Lets start by creating the load balancing group and that's done by simply putting the

name of the group inside of square brackets and underneath that the name of the host LBO one.


.


server we want every machine in our environment to be controlled and managed using the exact same process

even the machine that's doing the control.

So there's an initial bootstrap that's required when are installing and spel that can't be done with

ansible before ansible is installed.

But once we have ansible installed we want to use the ansible process to manage the rest of any configuration

that's required even on our control server.

So we manage every host the exact same way.

So now that I have all of these hosts filled out I'm going to save this file and just call it.

You can name it whatever you'd like for your local environment.

So now if we go here and type ansible list hosts all we still see that nothing is matching because we're

not passing this inventory file that we created.

So the way that ansible allows us to do that is by passing a dash parameter when we execute a command

.

So if we go into that Ansdell directory We were just in we can type ansible dash Hi dev lists hosts

all.

And now we see the host we've populated a few things to note about this.

One is the control host is just entered by hostname and by default ansible it is going to attempt to

S-sh into every host that's can be in the story since we're going to execute ansible from the control

host by default.

It's going to attempt to S-sh into itself which if you set up all the permissions and the key trust

that will work but it's it's unnecessary because we're already on that host.

So ansible gives us the ability to specify the connection type.

In this case we're going to change the ansible connection to local type which tells ansible just to

connect without attempting to create an S-sh section first.

And that's just for the control since that's where we'll be executing the commands from.

And that should have no impact on our ability to execute.

One last optimization is to address the fact that we have to now pass dash Hi-Def with every command

.

The precedence order that ansible follows is to first looking at c ansible hosts.

If it's not provided any other inventory information and that's set by the global Ansible configuration

file which is an the handsful ansible that TFG.

So here you see in this line inventory is at the ansible hosts and ansible allows us to override this

configuration by putting a file on our local directory and we'll use that to set the default tune Torri

to our local def file so that we don't have to tap type.

That's the idea of every time.

So we'll create another file and it will create the default section in that file and pass inventory

is the def file that's in our local current directory.

We will save that as ansible that CFT in our ansible directory.

Now here in our Ansible directory We have our ansible CFT file.

And if we run ansible list hosts all without passing Dasch I've dev.

It's now picking that up by default using that local override in the configuration file.

And we now can execute commands just like we would if we're using the global inventory but we have the

added benefit of being able to track or inventory in our code repo locally with our playbooks.

So with that we have our host set up next we'll look at different ways that we can select subsets of

that inventory if we want to target particular Hosa particular host groups.

So now that we have our inventory set up we can now start to manage those hosts with ansible.

However most of the time you're not going to want to run in your commands against all of the hosts in

your inventory.

You're going to want to select some subset of those hosts whether by individual hostname or likely by

a group or role.

And and that gives you a pattern syntax that allows you to select some subset of your report.

So let's take a look at some of those patterns that you could use.

The first one we've already seen and that's the keyword all.

So if we use our lists hosts shortcut again if we run ansible this hosts all we see that it returns

everything inside of the inventory.

We can also use globbing or wild cards.

So Star is the equivalent of using all.

And note here that I'm putting star in quotes if you're using the host patterns inside of the ansible

playbooks which we'll look at in the future.

All the quotes are not required.

But since we're using Basheer to run these commands anything that bash might interpret We need to escape

or quote first.

So in this case we're going to quote our Asterix here and that is equivalent to running an symbolists

house all these two are the same.

So in addition to all an asterisk you can use individual group names for instance.

You see over here in our inventory file we have a group name load balancer and that matches the single

load balancer and we could also test with our web server.

And you can see that both web servers are returned.

You can use individual hosts names you want to connect to a single host or run a run a command against

a single host.

You can use wildcards.

Even within the names so for instance app zero star matches tap one app too you can use multiple targets

separated by a colon.

So for instance if we wanted to look at the database group and the control group we could use a colon

.

Something to note is that the colon is being deprecated in Ansible So depending on the version of ansible

that you're using you may see a colon or you may need to use a comma in the future of the comma or place

the call.

It's the version of ansible that we have right now.


In addition if there are a number of posts inside of a group you can use an index selector to pick out

particular hosts depending on its order in the list.

So for instance we can use this host's web server which we know has two hosts and select the first item

which is the zero its index web server and that returns half a one.

And finally we can use negation to select everything that's not inside of a particular group.

So again we're going to escape this bang because we're in a bash prompt.

But if we say exclamation point control or bank control we want everything that's not in the control

group and that returns everything that's in the other three groups that we specify.

So these are the majority of the ones that I would expect you to use throughout your your time with

ansible.

The documentation has the full list.

And here I'll fill it up.

So if you go to Doc's ansible dot com in the introduction section there's this page on patterns and

it lists out all the different types of patterns that support it.

Basically what we've just gone through and a few others that you may find use of the end result though

.

The lesson to be learnt here is that if you use a structured hostname pattern or if you do a good job

of grouping your hosts into meaningful groups and roles inside of your inventory file then ansible gives

you a lot of flexibility for selecting the correct subset of those hosts that you need.

When it's time to execute commands against

That we have our inventory populated and we've tested different ways to slice that inventory up and

connect to them let's actually connect to a few hosts and run some simple tasks.

So here is a simple task and ansible this is happening.

And the purpose of the ping is just to test connectivity to make sure that Anspach connect to the hosts

.

Here we see the success for the ping against all the hosts in our group or in Ansible the task is the

basic building block of all execution and configuration.

The task is made up of two main parts the Montral and any arguments for that module.

In the case of pain we set dash and ping to to select the ping module.

And because Ping is a very simple module we dont need to specify any arguments in the case of thing

we just say ping.

And it runs.

Let's try a slightly more complex task.

Let's run a command hostname.

In this case we selected the command module so you can see here repasts dash and command.

And this time we had some arguments passed.

So we we pass a hostname and then our target pattern all in the cases command.

The parameter is a freeform text.

So we passed hostname and that that command hostname was passed to the command module and executed on

the host using using a command subprocess for most modules.

There will be some that accept freeform text like this but most of the time crammer's will come in the

form of key equal value pairs that are spaced separated.

We'll see examples of that later on in the next lectures.

Command is a special module in that it is the default Montral if you don't specify one.

So ansible dash and command is the same as running ansible a hostname.

Well let's look at the output of this command.

We see that for each host we get an entry back in this output.

We see the name of the host that was run the status in this case you see success and because we're running

a command it gives us the return code of the command.

This is the exit code of the process that runs on the host.

And then below that is the actual standard out from the run.

So in our case we ran the hostname command.

We would expect that to look exactly like the hostname that we've specified in the inventory and it

does it matches up in each case control returns control.

You think about what tasks represent.

In this case the command module is S-sh into the hosts and using the python module to execute some command

subprocess wrapping up the result giving us back that that status all tasks are going to have some sort

of return status even if it's not a pure command and you don't see a literal return code.

You'll see that there is a success or failure and that success or failure really is driven by the underlying

command and it's status.

So for instance if we a set of run set of running hostname we run been false.

We see that the command has failed on all those.

Now in falses a command in Linux that simply returns a non-zero exit code.

So our return code is one for every host and that's deemed a failure by ansible.

Even though we wanted it to run been false and false rants excessively in falses were returning a non-zero

Mexico.

And so we see that as a failure and so ansible regards that as a failure.

And you'll notice that there is no standard out for the been false commands so it's empty here.

Hello.

HOST.

Now these are two very simple modules the command module and the ping module but they're very useful

in terms of basic connectivity and troubleshooting when connecting to hosts and running ad hoc means

ansible has a large number of modules for many different applications and figuration items and you can

find that in the Montral index in the documentation.

So here I'm back at docstring ansible dot com.

And over on the left I've selected module index and you can see all of the modules grouped basically

by category.

Now in Ansible there are lots of core modules and there are lots of extras which mean they're not core

common functionality you'd expect to use in managing Linux hosts.

And just to pick out for instance how one that we just used here this command module section shows you

the command module here and by clicking on it you can see a synopsis or description but also all of

the parameters that are taken and which ones are required.

What are the defaults for those.

And this is true for every module that you would want to use.

So we'll work our way through all of the most common modules throughout the length of this course but

I would encourage you to hop to the documentation and just take a browse through the model index and

see what's available.

So then you get a good sense of what types of models are out there and the way that their shop should

So far we've tangible to run ad hoc commands against the hosts in our inventory.

This is great for live troubleshooting or for getting discovery or other information back for your hosts

live.

But the real power of ansible is in writing these tasks down and executing them in bulk against our

to manage our systems and the way that we do that in the animal world is by writing playbills now a

playbook is a file written YML syntax made up of plays and a play is simply a set of target hosts and

then the task to execute against those target those if you're unfamiliar with normal.

It's a simple markup language that's very common in software development.

We have a lecturer in the appendix explaining all of the details of the animal.

If you don't know YML it's fine will explain enough of it for you to use it when writing plays and playbooks

but just know that that's the official syntax that athlete uses for all of its playbook configuration

.

So let's jump to our editor.

I'll start by creating a new file

and inside of her and spool directory I'm going to make a folder called playbox.

I'm going to create a file hostname.

I am

now inside of the hostname file.

Will start with the standard YML opening of three hyphens in a row.

And the syntax of the play book is a list of place.

So each each item in the list in YML starts with a hyphen.

And there are two keys inside of each play.

One is hosts and that sets the target for this play.

Now we're going to say all those and the second key is tasks and this is also a list.

Now each item in this list is going to be a tasks that we execute.

In our case we're going to use that command that we use from our last lecture hostname a few things

to note about writing these tasks and side play ones one is unlike using the ansible command work.

Each execution is just a single task.

A play is a way to build multiple tasks that execute against the same target.

So if we wanted to run several commands in a row using ansible we'd have to type and edit the command

and exit impressive enter after each execution with the playbook we can execute this and it's going

to run multiple tasks in succession.

Also notice that we're using the command module and the same argument syntax and the way that we do

this and YML is in the task key we can enter the module colon and then any of the arguments.

Same way that we would inside of that cache with the quotation marks.

So we've basically taken that command line execution that ad hoc execution of the actual command and

weak ported it to a playbook.

Now the end result is going to be the same.

We're going to execute this command on all the posts in our environment but there are a few things that

we've gained by putting this into the playbook format.

One is we can easily extend this command by adding more commands without having to rerun and create

new entry points.

So now do we have a basis we can extend it easily adding more commands using the same target structure

without having to specify those targets.

The second benefit is that this command is now in a file that we can execute.

So we know that we want mistype mistyping execution be we can see what we executed last time by modifying

a file and that file can be committed to a repository.

We can put it in a good repo.

We could see how that file has changed over time.

We can track the changes we can do the changes and perhaps most importantly that repo can be provided

to an entire team.

And so the entire team can collaborate on the contents of what's being run and share the tooling that

manage just the infrastructure and it's really critical as we're starting to move to an infrastructure

code workflow or dev ops workflow is that we're looking at ways not just to run the commands and get

the end result that we want but taking a look at the process of how we get there and using tools like

get her other code repository version control systems to help us get there from here on out.

We're only going to use playbooks to execute changes our commands.

We're not going to run any more ad hoc commands like I mentioned before ad hoc commands are great for

troubleshooting or for getting information back and back to environment.

But for making changes playbooks are the way to go.

So that's how we're going to do it from here on out.

And the next video will execute his playbook and look at the playbook out

Now that we have our first play book let's go ahead and execute it.

I'm going to jump onto the control machine and my ansible directory and I will type ansible playbook

playbooks hostname yml. So

know a few differences about the execution that's as compared to running the Anspach man.

One is that we're now using handsful playbook instead of ansible and spool by itself was used Redhawk

execution and spel playbook takes a playbook as an entry point and you'll notice that we didn't pass

the dash cam or a dash a the contents of the MTA are really now contained inside of the playbook.

Right.

So the command and the arguments are inside of the playbook.

So we don't need to pass any of that on the command line.

And we also don't pass a target pattern.

So we we specify the entry point for the playbook but the target is specified really statically encoded

in the playbook here in the host key for the play.

We've gone from a dynamic interrogation using a Ad-Hoc module argument and hosts list to just execute

this playbook which has already defined those things in code.

Now note the differences about the output of the playbook execution.

First off we have the play itself which we ran against all.

And this first step is gathering facts and we're going to talk a lot about facts later on.

But just know that we have this additional step now where the first thing that ansible does is log in

to each and host and get a list of facts or details about that and host and brings it back so that they

can be injected into the playbooks.

And you can you can build logic into your playbooks using these details about the individual hosts.

It's not required.

It's optional in our case we don't use any of those but by default ansible is going to execute together

in fact step as its first step.

Each playbook execution.

Secondly we see that the task is executed and by default the name is given is created using the command

or the module that specified and some arguments so ansible is going to craft this name for us by default

and you can see Command hostname as is almost literally what we've typed into our playbook here.

And you can see that it was run for all five of the hosts in our environment.

Notice that the status now reports change back then we were running with the ansible execution.

That status was either success or failure.

Now we see that the status is changed for the execution and we don't see the standard out.

You don't see the actual name of the hostname being executed.

And this really starts to illustrate a shift in the thinking about the way that we're using ansible

rather than querying the host for some information and wanting to see that information live.

We're shifting towards a situation where we don't care so much about the actual value of the result

just that the execution happened without errors.

So we trust that what we're trying to do is converge on some configuration state and not necessarily

get back specific information for this for this particular command and here at the end of the execution

.

We have the play recap which shows us all of the hosts that were targeted during this execution and

a number of changes in the number of steps that took place and you can see we had the one change for

the command output and everything went OK.

Something else to note about this changed is that all we did was execute a command which report it's

an output but from Mansell's perspective it reports that as a change on the host even though really

logically speaking we didn't make any real modification.

And this highlights the fact too about idempotents and the danger of using things like command and Shell

.

Many of the modules and spools able to tell automatically Yes there was a change that happened or there

was not a change that happened.

I mean when there's no change you'll see it.

OK.

And that step will skip.

In our case running command it's really up to the contents of our command to determine if there was

a logical change on our system.

So ansible reports back I got some output.

We believe something's changed here.

There are ways that you can make the shell in the command more idempotent and modify that.

But from Antipas perspective it has no awareness only that the command run ran and at exit it successfully

.

That's why we don't see it as a failure.

But other than that the command is just complete and it's up to us to determine whether that really

made a meaningful change on our system.

One last thing we'll do here is modify the name of this command.

So by default I said that Anabelle gives a name to that to the task.

Now if you wanted to provide some more detail you could specify the name yourself.

So if we go back to the playbook instead of directly entering the command as the only key we can specify

the name key as a part of this task and give it some other interesting names so let's say something

like Get server hostname and we'll save that and execute executed again.

Now the contents of the command has not changed the underlying task is going to execute exactly the

same way but at least now when we execute it and look at the output in our terminal we get something

more meaningful to us.

So you see now that the task has changed from command hostname to get server hostname.

In this case command hostname is is pretty explanatory So that's that wouldn't have been a problem.

But in more complex scenarios rather than seeing just a raw shell command or some other underlying cryptic

command you can actually supply your own name sort of like a label or a heading that allows you to get

some some better references to what's happening in that particular task.
