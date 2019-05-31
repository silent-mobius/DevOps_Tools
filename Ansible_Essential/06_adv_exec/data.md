All right.

We've reached another big milestone in our journey of configuring our environment using ansible.

We've we spent some time making it work and making it right.

Doing some refactoring and making it more modular and supportable.

The last step in the journey is to look at ways to make it faster and that's to extract a little bit

more performance out of the way that we execute Aspel so we can save some time.

You might wonder why we're saving this to the end or why it makes sense to do that make it fast last

.

And there's lots of reasons for it.

One of those is that you don't you don't want to buy something that's going to end up getting refactored

away.

But ultimately the part of teaching this course and kind of going through these steps is I want you

to have a good grasp of the concepts as they are without a lot of extra logic and optimizations filtered

.

So you can see the core underlying concepts now that we're reasonably sure that that those playbooks

and those roles are not going to change drastically due to a big factor we can.

We can spend some time now optimizing and getting some more speed out of our setup in the future as

you start to use and spinning your own set ups you might implement some of these optimizations right

from the get go and that's fine.

But keep in mind you know you're going to spend a lot of time iterating and changing and it's good to

have a simple playbook set up that you can really focus on the content while you're going to that process

.

So before we dive into looking at ways to speed up the set up what I want to do is kind of get a snapshot

and see where are we now.

Like what's having done almost nothing to focus on it's speed and execution time.

What's the benchmark of what it takes to deploy our environment now and then we can use that as we kind

of make improvements to see what what gains we get as we as we optimize our stuff.

So I'm going to execute our three entry point playbooks here and just get some timer values on that

.

And then we can refer to these later on when we execute these start with the site Anmol and its execution

is really a no op.

We're at the point where our environment is configured exactly how we expect it to be.

So there's not really much that's going to change.

But having this understanding of kind of what the base time to wait is for even in no office it is really

helpful to understand where we're waiting as we exit.


execution of the site.

I'll do the same for our stacks.



a lot if you're running in production on DVR boxes.

So we've got our benchmarks here so let's get started on trying to get those numbers down in the next

few

Throughout the course so far we've executed playbooks many times and so you've probably got a good sense

of where it is you end up waiting the longest for these books to complete.

Now that we're relatively sure that our playbooks are stable and our code is not going to change too

much let's look at some ways that we can speed this up.

One thing you may have noticed is that during execution the first step is always gathering facts and

that's regardless of if we actually use those facts or recall that facts or those tidbits of information

from the server hosts the target hosts that will goes out and collects just in case we want to use them

as part of our playbooks and our roles and in the case of my sequel we did use that fact to configure

the secret listening address.

However in lots of other cases we're not making use of those facts.

However ansible still is going to go out and gather them and that takes time.

So one really simple optimization we can make is look for the places where we don't need to gather facts

and turn off the fact gathering.

It's really simple.

So let's look at Stack's status as an example inside of stacks stack status.

We have a number of quick checks that we want to run and we can turn off the fact gathering very quickly

with Gather facts false stories just like that for this particular play.

We're going to skip the fact gathering which should speed this up significantly.

Let's just copy and paste that down to here.

Now we can't do it here because we're actually using this fact if we skip the step the fact gathering

step.

We couldn't be sure that that fact is up to date or available.

So we'll need to leave the fact gathering and that's step in here.

We can remove it and here we can.

And so let's give it a whirl.

I'm going to run it with time which is just going to tell us at the end how long this command took to

run in our Linux problem.

All playbooks stack status.

And you see that we skipped that fact gathering step and go straight into checking the status of those

services.


So we've chopped up some time from our initial run that we saw when we were doing our initial checks

for for the default we're redoing gathering facts at every step.

So almost almost no work we did there.

We really were just calling out unnecessary steps and we get immediate gains and we can do that across

the board.

So I'm going to take this gather facts step.

I'm going to apply it to our stack restart hosts

again skipping the removal for our database because we need it.

There are hostname which we haven't touched in a long time.

Sorry about that little buddy.

We're going to update you as well and in all of our roles we check the database.

Susan we checked all the roles here we can check the task list.

No not on the task list and the actual checkpoint playbook.

So control you don't need the fax database.

We do load balancer we don't need the facts and web server.

We don't need the fax.

So just like that we've gone a lot awful lot of our execution time with basically no downside with that

we'll look for other ways that we can save some of our execution time with our necks like

Now for our site.ml and roll configuration playbooks will notice that a lot of our time is spent waiting

for apt to update and install.

So even though our packages are already installed though we're still waiting for that app cache to update

and we have to pay that price each execution even though we know we're not going to do the upgrade because

we're using status present.

So that update cache yes line gave us the safety of knowing that on the initial install the very first

time through we get that cache updated.

And that's that's still required it's still necessary to have what we have to pay for that overhead

every time we execute now after the initial installation.

So that's a bit of a problem.

And one thing you can do now is as you start to go back through and look for places to optimize sometimes

a good idea is to pull up the docks and just jump through and see what's available to you for this type

of work.

So I I have the the docs page here up for the aft module.

You can go to module index it's in the packaging section apt.

And as we scroll through here we see a few things one of them here right at the very top is cash valid

time.

If that cash is specified in the last run is less than or equal to this time that we set up that cash

gets skipped.

That sounds like exactly the kind of thing that we want to use here.

So we're going to implement our cash Breillat time to reduce our overhead and skip the unnecessary cash

updates.

The other thing we're going to do is take that cash update from all of our playbooks were distributed

and we have to run through sequentially and move it into a common step where we can parallelize that

across all of our hosts at the same time.

So in a small cluster like this no big deal we can have five posts updating at the same time and we

get to bear that cost if an update is required.

All at once rather than staggered one after the other five times in a row basically.

So to do that let's go to our site Hammill and add a new play here at the top.

So before we start any of this we're going to create a new play.

This is going to look just like the plays we've seen before before we started our roles work become

as true other facts is false.

We don't need that here

tasks.

The name I'm just going to give it up.

They have cash

up they cashes Yes.

So we don't actually want to install a package we're just turning off that update cache and we're going

to set our cash valid time.

You can use whatever you like here.

I'm going to give it a nice generous.

Eighty six thousand four hundred.


All right.

So that pools are up they cash.

We're going to hit all the hosts at the beginning at once which means we don't need it and all of the

other playbooks so we can go back and remove it from all of our tasks inside of our roles.

So here believe that

same inside of control

got to here we can remove

.

To here inside of our next world.

All right.

So let's give that a quick try

playbooks YML right there.

So we've got our playbook execution time time down under a minute.

And if you noticed as it's going through you probably noticed a little bit of a speed up at each step

that we're trying to install.

It still has to go out and check that the package list the app list of installed packages so it's not

down to zero but it is much quicker this time around because we're not doing that update.

They cashed out every

All right so we're speeding up our execution times which is great but psychoanalysts analysts still

taking about a minute even more performing.

No new changes it takes that long just to tick through and check that everything is still exactly the

same as last time on the grand scheme.

A minute is probably not that much time but it really does add up especially if you're doing a lot of

iteration and even more so if you know for a fact that a lot of these hosts haven't nothing changed

and you're just making changes let's say on one particular role or one particular post.

I really don't want to wait for the entire site on the table to go through or even in the case of a

particular host by itself.

So you could use that say the web server that to just target those but maybe one had a hardware issue

you had to bring it down and provision a new one from scratch or you decided to add a bunch of new hosts

into your inventory.

You don't even want to apply to all of your web server hosts.

You just want to target one particular host inside of the group.

So how can we tackle just targeting one.

Well you could change the host line inside of a playbook that hosts pattern and run it that way.

But that's not necessary because ASP will gives you a way to limit the execution to some subset of the

full pattern match that's available.

And Tori So let me show you how to do that.

So it is very simple from the command line we wanted to run site that animal for instance we would normally

just run ansible playbook I thought YML.

There is a handy dash dash limit option show option.

In this case you can pass the same type of pattern string that you've been passing before.

Let's say app one.

So you notice that we get to use our update at cache which was applied to all before and by we would

have missed out on that if we'd just use reps or reenable so we get the full logic that we want to apply

to our cluster.

But it's just scoped vertically sort of at that particular.

So every task from top to bottom that matches that host gets applied.

So we get our update up cache.


just against that post top to bottom.

And in this case it finishes much faster because we're just using that one.

So limit is very handy.

It's very handy way that you can use the same entry point you can you cite thought YML what you're starting

to get used to executing.

You can sniff out this particular host or this group of hosts and just apply it to those.

So you don't have to wait for any of the extra votes to get applied if you know for sure he just needed

on one particular host.

That's a great way to save a lot of time when you're doing refactoring work or iteration or some some

maintenance in your environment.

We see how we can select a subset of our hosts using the limit parameter.

When we do this we apply all of the tasks in the playbook to the specific hosts or hosts that we want

using anti-Paul we can also select a specific task or set of tasks and apply that to any number of hosts

in our environment.

Sort of a horizontal selection across our all across our cluster and we can do that using tags.

So in Ansible tasks are arbitrary values or labels that we can apply to tasks or playbooks that we can

then use to filter and select out just parts of our our playbooks roles that we want to apply to our

close cluster when it comes time to execute.

So let's start with control which is a really small role and I'll show you how these tags work

inside of our tasks.

Here we have a single task and I'm going to split up the tasks because it's flexible you can pick whatever

system you like and how you use and how you segment your tasks.

I'm going to break them up into the sort of pillars that we talked about before and of the four main

sections of doing your configuration management packages services so general system config and then

application or road configuration.

So in this case it's another key that we add to the task here and you can specify tags and then supply

a list of tags.

And for personal preference instead of specifying it has a list of one item per line.

I'm going to give gives a square bracket and put the name of the tag inside of that list there.

So having done that we've now said that this task is a member of the packages tag.

Over here on the command line we can use a special parameter to list out the tasks that are available

very similar to the list hosts that we saw before with ansible.

We can do list tags.

And that's going to go through and pick that playbook that we selected in.

And just look for all the tags that it can find and you can see here now the top level plays themselves

and get tags.

And we can also see tags applied to individual tasks we see out of all of the plays that are going to

be run.

We've got the package's tag available for this control control play.

So now if we wanted to run just that particular tag we could say ansible playbook site that you will

test tags and then the list of the tags that we want to pick out.

So by executing that we see that we skipped the initial app cache update play that we added and recently

at the very top and it only selected down this particular task from the control post.

Notice we didn't have to us to limit down to it to this particular house because that's the only task

in the entire playbook an entire set of hosts that were going to apply to that has that package is tag

.

That's the only task that gets executed.

Ansible also gives you the ability to sort of invert that logic and say just skip this particular attack

.

So instead of selecting tags we could have said Skip tags and it would have run everything except for

that particular task and this is a.

This is a handy thing because we talked about how the packages really are commonly the longest waiting

tasks the when you have to wait on the most.

So if you get to a point where you're executing lots of stuff and you just want to skip just all the

package stuff don't bother doing any package checks.

I know nothing's changed.

You can do it skip tags packages and zip right through and jump right to the other things that you need

to do in the playbook.

Without knowing all of those particular tag names

so let me go through our playbooks here and I'll spend just a few minutes.

How quickly adding CACs to all of our tasks that we can see what that looks like when the entire set

up has tags set up properly

.

Ok one last place we shouldn't forget is our site that in Yemen which has our task and that's part of

the package script.

OK.

So we've got all that done it took a few seconds but now that's out of the way we can leverage those

tasks.

I want to go back and use tags again.

Now we see that we've got our packages configure Service System scattered throughout our entire setup

so now when it comes time to execute.

Right.

We've been trying to optimize the full run by we're moving we're moving tasks they're going to run every

time at this point.

If you have particular things that you know you need to do you can basically go in and select that tag

can kind of snipe down that that particular task you need executed and you don't have to wait for everything

else.

Even if it is going through quickly you know for sure this is the this is what I need to have run.

And when you get large clusters and large actual set ups that's really a great way to save time.

So just as a test for this I'm going to do a ansible playbook execution for site dot yml.

I'm going to skip tags packages because we talked about how that's really one of the things that takes

too long is for us to execute.

We'll see how long it takes here.



.

So it's a huge savings here.

We've got a lot of time back just by knowing what it is we really need to execute.

So one last note you can apply multiple tags to individual tasks.

So it's it's a list of tags you supply for each task or play.

And so that task can be a member of multiple tags.

And when you select tags if that task is included in any one of the ones that are allowed through the

filter then it will get picked up and run tags are very flexible which means they can also get out of

hand so you don't don't feel like you need to add a tag to every single task or every single play.

Use them sparingly kind of as you see a need to speed up your execution.

I just remember keep things simple.

Be kind to your future self and remember how how hard it might be to maintain a lot of that once you

once you had a lot of that extra logic.

One of the great things about ansible and the output that you get when you execute is the play recap

at the end which gives you a quick glance you can see as anything changes anything failed especially

after a long play like execution.

It's great to be able to see that at the end.

One of the things you may have noticed is in several of our playbooks we have things that are always

going to register changed because of our use of the command and shell Montral.

It would be great if we could silence those changed or affect them some way so that when we look at

that play recap we really do get a true sense of everything's good and Grainne just ok or something

failed.

It's red or truly if we get the yellow changed that meant something had to be altered in the environment

.

Also ansible does gifts give us a way to do that and supply our own eristic to the executions so we

can say here's when I say I want you to tell me it's changed or not.

So let me show you how you can do that outside of the playbook.

We're going to do this first with the stacks that this playbook.

And let me execute this one so you can see what this output looks like if you haven't ever run recently

.

And here at the top you see varify Internet service is running our service engine status command and

it comes back has changed even though it's it's really OK and that's really because of the exit exit

return code of that of that command and the way that animal interprets that return for the command module

.

If that were not OK we would we would have gone and gotten a return could have one and we would have

seen it fail there.

So in our case what we really want is failed if it's not running.

Just tell me ok and not changed if it is running and to do that we can tell ansible how to interpret

how to interpret the output and when to determine something is changed.

And we do that with the change when parameter and what we supply here is some sort of boolean value

or some expression that reduces to a boolean value.

And ansible will check that line and determined to change status based on what we supply.

So the simplest case here we can just say changed when false.

And what that's telling ansible is if you need to decide if something has changed we're not going to

give you the option of changed.

It's always going to be never changed either.

OK.

Or failed.

You never get to say it changed.

And we can copy that down to all three of our command execution lines.

And this just ensures that we're either going to get an ok or failed when we run.

So let's run that again and see with them site.

OK.

So now we see everything is green everything's OK.

And instead of having to interpret that change and wonder did something actually change there.

It's a quick way to use that play recap a little more efficiently.

We do have one more command execution that we've got snuck into our nginx roll.

So let's dive down there and I'll show you how we can amend that to get some better better feedback

.

So here we're running this get active sites and our LSS a shell command so that's kind of register changed

basically every time.

So in this case instead of doing a changed one false let me show you a more complex example.

We're registering active which is a line by line list of all of the active sites and we get to use that

register inside of the changed changed when expression.

So I'm going to say when active dot standard outlines which we've seen before is show me the output

but give it an array one line for item in the array.

So when acrobat standard outlines the array is not equal to the site's cheese.

So because this is basically a Python expression that's being evaluated and samples written with Python

so effectively this when command is a Python expression being evaluated for truth or for true or false

unless we're saying does this array that has a list of active sites equal or not equal the keys from

the sites that we have in the defaults.

So here we're going to form an array out of each of the keys.

So in this case my app is basically built to erase each of them should have a list of sites that we

either want to have configured or are actually activated and configure when they're not the same.

We think something is about to change here and that's when we trigger the change to win.

So having this line in here.

We'll see what that looks like when we do an execution.

And we do that now.

So I'll run the site and will and we'll use some of our recent features.


So here instead of seeing that change line we get it.

OK.

Quickly let me show you what this would look like I give you a quick demo of our testing it to make

sure that it works when there is an actual change.

Let me just add a demo a dummy value here for a second.

Second site Save that

is.

And here are good active sites actually register that change because we had a site that we wanted to

do at here.

And if I delete it it's

changed again for the removal one last time.

Back to all green Nothing had to change.

So now we've got our play recap showing us exactly kind of what we want and not getting it.

Giving us a change for something actually hasn't changed.

So a few final notes on this here

in addition to that changed when key that we have here there is also failed when so if you want to apply

the same sort of logic you can do that using failed when and determine what it is that ansible tells

you when something has failed and it's not just driven either by the built in module logic or the exit

code of the command.

For instance

You've looked at a few ways to squeeze the most performance possible out of our ansible playbooks and

really get the wait times down as we're executing all the little tips and tricks and things that I've

shown you are basically universal for ansible in the sense that regardless of your application or environment

or what you're setting up you can use these principles to get faster execution something or time back

.

There are some more ways that you can modify your install setup to get more performance but because

there are more environment specific They may not apply to you.

I've held them off here to the end so I mentioned earlier that ansible is a python application and that

it uses S-sh connections as its as its transport transport pick and isn't.

Python has a built in implementation called Param ECO and for old versions of ansible or S-sh that might

be installed open S-sh perma Kurmi it was sort of the default fallback that it has sort of lowest performance

if you're using a more recent version of Open S-sh.

There's a function called Control persists which which allows the S-sh client to reuse connections if

they're rapid back to back then reduce the overhead and an unnecessary connection times.

And if you've got a newer version of ansible and open S-sh and all that will you use it automatically

.

So you'll get those benefits without having to configure anything on top of control persistent S-sh

ansible also has a few other ways that you can tweak your setup to get even more performance.

Let me pull up the docs.

You can see what those look like.

So here on the documentation page under the special topic section you'll see that there is an accelerated

mode link and so it it right at the top that talks about the fact that you might not need this.

That's because a newer version is there's a new concept called pipelining which uses some S-sh functionality

to reduce the amount of transfers that are required.

So it's all under the covers.

You don't really have to do much in terms of the implementation but you do have to tweak a few things

about your environment.

I encourage you to go to the stocks page so you can research exactly all the details of when accelerated

mode might apply to you or when pipelining might apply.

But that's the quick summary is this pipelining is best if you can use pipelining because of the certain

versions you have and the ability to turn off require TDY in your environment.

You should do that if you're not able to do that.

Or if you're running certain versions of Linux that don't support pipelining then you can fall back

and use accelerated.

So I'm not going to go through all the details of how to configure accelerated mode because it's not

the recommended way to do things but you can find all those details on the docs.

I'll show you quickly how to set up pipelining if that's something you can look at to your on the control

machine.

If you look at the animal configuration that's ansible that's the animal ants will see of G there's

a section for S-sh parameters.

And down here we see that pipelining is disabled by default.

And that's because of the requirement around modifying FCC to to disable required.

So rather than change you to here is what we've done in the past when we need to edit the ansible config

is to go to our local.

And we'll see if we can do that again and we can set up an override to enable pipelining

pipelining etc..

So in my case and it have been to that that's all that's required.

If you look at the suitor's file and if you're not familiar pseudo.

Visudo is as a way that you can edit that file safely not lock yourself out under this default section

some distributions will have a defaults required to your line which basically says you cannot sudo unless

you have a true T.T. line.

And that's that's a problem for ansible.

So in certain distributions you need to go in and disable that.

You can comment it out or you can use defaults not required to do something like that to disable it

.


And so for large code bases large environments the gains that you get really can add up over time especially

if you're running ansible a lot over many hosts.

It might be worth investing in that your environment.

Like I said I saved it to the end because it's really an optimization that that will benefit you a little

bit as you're just starting out.

So it's not something I'd recommend just going and doing off the cuff unless you found that that's something

you really need for yourself.

So give it a try.

Check out the docs and see if pipelining or accelerated mode might be for you.
