As you're building out your ansible codebase and populating your roles and playbooks inevitably you'll

find yourself in a place where you hit a dead end and you can no longer run ansible and you've configured

yourself into a corner.

Oftentimes it can be traced back to an ordering or dependency problem between the tasks that you set

up and the discrepancy between the initial configuration and the re-application of that configuration

.

So if you think through this process of applying ansible we're oftentimes in the devs set up as we're

iterating through we're building up layer by layer and we're not going back and starting from refresh

environment.

And when we do that we can sort of introduce kind of hidden dependencies that we haven't made explicit

in the playbook.

Can come back to bite us and get stuck.

So there's an example of this earlier on in the lecture that we that we corrected so by reordering and

I wanted to kind of dive in deeper to that example and talk through exactly what happened and in some

ways that as you're doing yourself if you run into the situation some tools you can use to get to get

unstuck and keep moving forward.

So here's the example here we're looking at the my simple task list.

We initially started with laying down our service because we wanted to get it started in Naples and

then after that we came back and did some reconfiguration of my sequel and when everything is going

great.

This is great.

And you never run into a problem here.

But if you think logically about what's happening here we can run into the situation where we say we

want to start the service and we want to make a change to this to this configuration.

And in the case of my sequel if this Miss Miss if this configuration is incorrect it will prevent my

sequel from running.

So we are up and running.

We make a change.

Let's say we botched the change because of a typo.

We had an error.

Here we restart my sequel and it fails so because the configuration is incorrect.

OK no problem.

We had a typo.

Let's go back and fix that.

We update our playbook.

We run the playbook.

We hit this line.

Here again it says state is started.

So we're telling ansible that you need to make sure that that process is running before you continue

and because of the bad config error out fail the playbook stops and we never get to make it back to

the point where typo gets fixed and reapplied.

So now we're dead and we're at a dead end and we've now we've not gotten ourselves stuck.

So how do we solve this early on in the course we solved this by moving this.

This particular task.

That one.

So that's kind of a logical solve and I'll talk through more about how to think through some of those

things in a second but some other things you might think about doing so.

One of them is you could manually go and log into that post and fix whatever the problem is to get it

on stuff right.

So if there's a typo in this at my sequel my daughter CNF file you might log into the DB host manually

edit that file to get it unstuck and get moving.

And as a last resort that's always available to you.

But I encourage you not to reach for that immediately because we want to build up the robustness of

our playbox And if you do a lot of manual tinkering and working around or working around handsful you'll

probably find yourself in a place where you don't fully trust ansible to do the job for you because

it's going to be missing some corner cases like the one that you just fixed yourself to something else

you can do.

Is is is kind of help and will get past it temporarily.

So in one case we've come in and simply comment out this requirement.

Right so we could say not in the end game but this time through.

Don't worry about the sequel service.

You could run it.

It would fix it.

Then you could uncomment.

So it's a little bit better than going manually and touching the system but still we're having to do

this tinkering and introduce sort of a transient state to the state of our label.

Another option which is very similar is to introduce a ignore errors cloth's.

So here we could tell ansible to run it.

But in case it fails don't don't bail out.

Just ignore the fact that that failed and keep on truckin.

So this would show back as a failed step as we do the execution.

But it would allow us to allow us to then hit the next config step which would correct our typo.

Restart my siecle would be app back up and running but again we'd have to come back and remove this

line because in the long term we really do want that to tell us if there's a reason that may sequel

.

Sequel really cannot start.

So last thing and it's the way that we fixed it earlier is to think through kind of logically what's

happening here right.

And because there is so much you can do with ansible there so many different ways at these tasks into

play there's no hard and fast rule that I can tell you.

Make sure you do these things and in this case there's kind of a logical rule that we want to make sure

everything is configured before we force start that service.

So moving down this to the bottom this makes a lot of sense I think because you're saying ah is a good

likelihood that I'm going to be changing configuration.

Let's not try and start the service or get it up and running again.

Unless I've had a chance to get that configuration.

So that's kind of a general principle.

But other than that it really is going to take you understanding your code your application your infrastructure

maybe some trial and error to get unstuck but it's something that you should work through.

And I do encourage you to work through it using ansible itself rather than going through work arounds

and other methods by investing the time and getting your playbook to the point where you can really

trust it.

That's going to increase the likelihood that when you when you need to deploy something or you need

to fix something that you're going to reach for ansible and trust that it's going to be there and you

don't find yourself in this no man's land of all we've got this playbook that we've written but nobody

wants to run it because we don't know what it's going to do.

We can't trust them to take care as

As you're developing you are ansible playbooks or roles or if you get to a point where you you're executing

and you want to troubleshoot a particular task that's giving you some grief.

A lot of times you want to focus in and just run that that one part over and over and over again.

So rather than going into your playbook and heavily comment commenting out lots of things or having

to go through lots of hoops to get that one task kind of into the spotlight you can use that command

line options and that will give you to just focus and sort of select here's kind of where or what or

where I want to run.

Here's where I want to start on you story how those work.

So the first one I want to show you is the step parameter.

So for instance if we run ansible playbook site and we'll step

rather than simply executing each command and turn it ansible prompt us and ask us for each.

For each task do you want to executed yes or no.

So we can say yes this one.

Please go with it.

And now it's going to go ahead and run and execute that in the command and in the background and we

can then step through a bunch of tasks that we want to get to the one we want.

We can quickly like no no no no no let's let's just say we just run this one this run this one which

gives us the ability to kind of hop and decide which ones we want to run.

But it also allows us to execute a command and have sort of a halt that then says Okay that one was

run if I need to go troubleshoot something I can hop into that server check the current state of the

make sure everything's in place.

I may figure out why this particular value is not getting configured the way it needs to be hop back

into our terminal that's still waiting for us to go.

And then we can we can hit the next the next command allowed to continue as as normal.

So I had to do that once this particular task finishes.

So really this is a great way to kind of segment up your your playbook without having to inject manual

pausing prompting comments into the playbook itself.

So now that we've decided OK I was focusing on that task I'm satisfied I've done my troubleshooting

I figured out what it is that's a problem we can can kind of Control-C out of this and just break it

or we can tell as well just to continue as normal and their ansible goes about its business as if it

was just a normal task task execution.

So that's that's the one that's the first thing I wanted to show you is step one is going to break out

of that.

And the other thing is to actually pick a place that you want to start rather than start at the beginning

and step step step step.

Kind of skipping through to the point where you really want to start you can jump into a particular

place in the playbook so Ansible playbook citing and will have a handy list task command very similar

to our list tags and lists.

Host we get this output of every single task that needs to be executed before this playbook is finished

so we can use ansible playbook start at task and give the name of a task.

Let's say we want to start here.

Copy and paste that in Asia in Ansible is going to skip back skip down you notice it skips the play

control play database play and hop down to Web site replay to that particular task.

When the startup so this option right allows us to skip through a lot of content if you kind of mash

this together with things like limit and tags and step you really have the complete tool belt where

without having to modify anything about your playbooks they're all left in the sort of desired end state

.

You can go through and slice up and select particular tasks started sort of different tasks.

And this is one of those things that if you can add these tools to your toolbelt you find yourself iterating

through and getting those actual play books to their robust state.

You know much faster because of less time spent waiting for ansible to run on stuff that you don't care

about quite some time.

So we just saw how we can use.

Start at least tasks that if we need to jot down into the middle of our playbook execution which is

really handy if something fails and we will we don't want to start from the beginning all over again

.

We just want to jump down and kind of cover from our from where we left off.


for some reason two or three of them aren't available.

So how do you go back and decide which of those you want to rerun against in a large environment that's

hard rather than coming down all the names and sort of building clustering groups or using a long limit

string and still keeps track of this for you and lets you know which hosts have failed and allows you

to recover on those hosts in particular.

So I purposely failed in the app to host and we're going to run our playbook against our cluster to

see what it looks like to recover a failed host.

So ansible playbook site that YML and I'm going to skip the packages tag is to speed this up

.

So we see here that app to failed with an S-sh air and into play recap we now have this handy little

message that says to retry use limit at home ansible site retry.

Let's take a look at that.

And so what was done for us was create just a simple text file that's available in our home directory

with the list of the hosts that didn't make it all the way through long the last execution of the site

playbook.

So what we can then do is let's say we figure out a way to get our app to host back up and running on

the next execution.

We can run ansible playbook site enamels limit and pass these details here.

So rather than have to figure out and build our own limit list of the ones that didn't make it and didn't

go through or execute against everything and waste more time and still gives us this handy shortcut

to say I kept track of the ones that didn't make it obvious when you use these next time through.

So in our case we're still down here because I haven't restarted up to here but in a live environment

.

So you take some time and get all these hosts that failed back up and running.

You don't have to go back and start from scratch.

You can reuse your last event using the retry retry file that's created for you and the limit parameter

So far we've looked at ways we can slice up our execution and select a subset of our full playbook either

a certain number of posts or a certain number of tasks.

In the end though whenever we hit enter after executing our playbook all those changes go out live immediately

into that production system.

And there's often times as we're developing or iterating or about to deploy some changes we want to

get a little more assurance that what we've typed is really going to work before we go out and target

a production box.

So one thing you can really do is have a DAB in a stage environment but on top of that there are other

ways you can get good feedback before you end up pulling the trigger on a final execution.

So I want to show you some of those commands that you can use before it comes time to do a real deploy

.

It's the first of those is the syntax check.

And this is a static analysis check.

It just looks at the file and says is this valid ansible.

There's no no targeting end host there's no execution.

It's just a static check of the of the playbook overall.

So you can use ansible playbook syntax check and point it out a playbook.

It is a really quick parse and just tells you yes or no there's problems that it sees.

So just to show you what the output looks like if something were to fail.

Here we see that there's a Yamhill error on site.

I will because this include needs to be part of an array.

So that's that's the kind of what you're going to see it's really just sanity check that I make a typo

from the B-M will ansible perspective and something else to know.

We entered the site on Yamla as our entry point here and so we're getting the results back as playbook

site YML but it's going through a parsing a lot of under underlying tables right.

So if you're going to do a lot of the syntax checking it may be helpful too to iterate through and pinpoint

to get more meaningful error messages back.

That's the way that we can check the syntax is valid.

And that's a great check that you can do pretty much anywhere.

So even in a C I set up or locally on your box even if you don't have a target and host that you can

hit to test.

You can do a syntax check which gives you a little bit of confidence that what you're typing at is valid

and I want to go to work.

So a step above that is to actually do what's called a dry run and that allows you to to execute the

playbook.

But instead of going to actually make the changes able to stops and record what it thinks it's going

to do or what it plans to do when it comes time to execute the change.

So to do that instead of syntax check we run a check.

So this should come back much faster because there are lots of things that don't need to be done actually

moved by just a record of what action needs to be executed.

And you get those results back.

There are lots of limitations about check.

Not every module supports the ability to understand what's going to happen if you think about it logically

if there's any dynamic data that's required or if there's something that's that's relying on some previous

step to actually execute to get some standard out back for some logic to run the check is not going

be able to do simulate that logic for you.

It's really just simulating step by step it is going into the host and figuring out some of the current

state so we can figure out do I need to update this package do I need to copy these files.

What if it decides it needs to copy those files.

It's not actually going to do it.

It's just going to report back what it thinks it's going to do.

So this is something that is great that you can do just before you need to do an execution.

So if you're if you're just getting started with ansible you're kind of getting up to speed.

This is a great way that you can execute a check against the live environment that you're about to run

and you get a sense that if something is going to fail before you actually go to do the execution.

And here we see two things change that's kind of indicating Here's the stuff that I think if I were

to run this right now these are the things that I would have to actually change.

So it's a precursor something important to note about both check and syntax check is that when you're

running commands with the ansible playbook command you always have to provide some sort of inventory

.

So even in the case where you're running a syntax check and you just really want to check the contents

the inventory file has to be populated even if it's empty.

So if you're doing this for let's say for a c I set up or on it you know on Check-In you want to run

syntax check on every commit to do.

Sort of like it's a unit test for ansible.

You need to provide some sort of dummy inventory file.

Commonly what I do is just create a file that has one entry localhost ansible connection local and past

that it was Dasch I said that that requirement is satisfied before the syntax check or the check goes

through.

So with that you now have some tools that you can use sort of even before you ever get close to deploying

it in production and starting to leverage a lot of those other tools.

We've talked about hopefully that gives you some more confidence that you've got a workflow that you

can use and get feedback earlier on in the process.

Is this ansible code going to work.

Is it doing what I think it's going to do.

As my environment in the state that I suspect it's suspect it's an

So in the section we're looking at ways that we can troubleshoot our playbooks in our environment as

we're writing them and tools that we can get more information back out of both the environment and the

state of our playbooks as we execute them.

So one of the things the things that comes up a lot is the need to get at sort of the data and the transient

state inside of the playbook has it's executing.

And this really becomes a problem as you add more logic and more conditional paths through your playbooks

as they get more complicated.

And one example of that you see here is when we're looking at the nginx module we decided to shell out

and get access to something that's lived in the environment and use that to make a decision later on

as you're going through this and you're developing.

Sometimes you need to know like what is that.

What does that output look like so I can then write my next task in one way you could do that is to

actually log into the host and execute the command.

But you could also do it from ansible perspective to see what the output looks like to get get at those

results.

And the other thing is with lots of complex variable schemes and with defaults and overwrite Sometimes

you just want to know what's the value of this variable going to be at this point.

And to answer that question you can use the ansible debug module which is a very simple module that

just spits out some message or the contents of some variable in the middle of the playbook.

So to give you an example here and this nginx module says nginx role we have our task list and let's

say I want to know what are the standard outlines going to be by that.

Before we get here so I can make sure that when I'm writing this next task it's in the right format

so I can type debug.

And you can type a message if you just want to enter just kind of a print print up to the terminal that's

a static line of text but you can also pass a VAR equals and then give it the name of some variable

value that you want to have printed out.

So in this case I'd say let's get four active standard outlines and get that printed out to the council

OK let's execute that.

So we can see what that looks like.

I'm going to start right at this one here saves time.

OK so here's our debug here.

And we see it injected here and the output is simply we wanted to see the VAR.

Here's the name of the bar.

And then here is an array with one entry in it my app.

So that's that standard outline.

So then we could use this logic to then say OK well that's what it's going to look like it's going to

be array with my app.

And then we could use that in the next task.

As you can insert these debug statements you probably want to just leave them in Azer iterating and

developing not for actual production use unless there is some reason why at the end of your at the end

of your and simple playbook run you want to collect some values together of things that that were changed

or some particular values you can use Debug to sprint to print those out along the way.

Something else that's handy like I mentioned is getting the state of the variables so we used DB name

for instance we know that we configured it back in our in our group or is all set up so we could see

how is doing in this demo at this point which is really handy if you have that default plus bars overrides

look at the roles layer plus maybe an override of the group as layer and a dash.

I call it if you get to the point where you have to have that level of complexity and your variable

set up var is great.

And another thing you can do is is if you really get stuck and you need to know what's going on in your

variable space B is to print out a lot more information about what's what's currently in the vars from

the top level and then work your way down from there.

So using using the de-value you can just say you know I need to kind of take a peek and if you're a

software developer then debugging is a fair concept for you.

But if you have if you have some things you need to figure out about what's happening inside of your

playbook then I think if you've ever tried
