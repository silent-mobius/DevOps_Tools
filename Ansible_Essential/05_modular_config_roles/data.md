Before we wrap up our discussion on the roles I want to introduce you to the ansible galaxy and discuss

external rules.

So here I have an animal galaxy Web site pulled up its Galaxy ansible dotcom galaxy is an online depository

for roles that other people can contribute and you can download and use.

It's very similar to the puppet for the Shepp supermarket.

If you're familiar with those ecosystems if you have a common application that you want to implement

there's a good chance that somebody else has already provided an implementation on the Galaxy that you

can go down and just download and install locally into your Ole's directory and use as is with a little

bit of customisation there structure just like the roles you've been you've been creating.

We've been working with that's just made by somebody else and offer it up online.

So here you can see here how you can use the animal Galaxie command to install the role.

There's also the ability to provide a list of roles inside of her requirements file and having a galaxy

to install multiple at the same time.

We're not going install any rerolls here in this course but you can check the docs here on how to do

that if you're interested in trying it out.

I do want to show you some of the rules here to give you a sense of what that looks like.

Since we've been working with nginx I'll search for an engine next role and and pick one here that we

can find.

So here's one that's got a seems to have a high score.

So there are advantages and disadvantages for using external rolls especially off the galaxy.

So I want to give you some indication of things to look for if you choose to explore this as an option

for getting up to speed really quickly.

So one is How long has the has the world been around and does it have a high rating.


couple of months.

That doesn't necessarily say that it's good quality or low quality but if or if the role has been around

for a long time and a lot of people have used it and raided it there's a good chance that it's stable

kept up to date and you can kind of join that bandwagon and use it and kind of trust that it's going

to take care of your needs because you're not the only one Usie and in testing and providing feedback

to the author.

A second thing to look for is how much coverage does it have of the underlying application.

And what I mean there is if you think about nginx and all the different configuration items you can

change all of the different complexities and that in DOS config files the roll is going to wrap all

of that and present just the variables to you to configure.

So if there is a configuration item you need the author has to plan for that by putting it in the template

and giving you a way to configure that.

So if it doesn't have full coverage of the underlying app you may not be able to configure a certain

role a certain part of that nginx role without going in and monkeying with the role yourself.

That's really not ideal.

I mean the whole purpose of using a roll from the galaxy is that you don't have to mess with the internal

details of how that rolls configured.

You can just use it as a black box.

So it's really a question of how much customization do you need.

How much are you leveraging all that flexibility and the parameters inside of that particular application

and whether or not this role that you find gives you the ability to leverage those features.

And one last thing to consider.

How actively is it maintained against the underlying app so you want to see regular updates as nginx

versions come out and are in are available for great new configuration items will be revealed.

Old ones will be deprecated and the role author needs to make needs to update the role itself so that

you get access to those new config items and the old deprecated ones are marked as such are provided

with backwards compatibility and all of that's really up to the author but it's up to us the consumer

to make sure that that's happening and you can trust that if something new is coming out you get access

to that and it's available to you and you don't have to go in again and modify the roll yourself to

suit your own needs.

So in the end even if you don't choose to use public rolls off the galaxy you can maintain your own

roles within your organization have a sort of private repository of roles and use a lot of the same

tooling to get access to that.

You know in the end we've talked about the main advantage of roles is to minimise the duplication and

maximize reuse if you some encapsulation.

So it's up to you to kind of find the balance point within your Horrigan of getting the speed and getting

up to up to speed really quickly if you use something that's off the shelf so you don't have to build

it all from scratch.

But at the same time getting the flexibility and the stability that you would want to support your application

.

So that balance point is going to be different for every person everywhere.

I'd encourage you to take a look at the Galaxy see what roles are out there and maybe give one or two

or try to get a sense of how it might look for you.

[smobius@vaio 04 Modular Configuration with Roles]$ echo > tmp
[smobius@vaio 04 Modular Configuration with Roles]$ for i in *.srt;do cat "$i" |grep -Ev '[0-9]+' >> tmp;sleep 0.5;done
[smobius@vaio 04 Modular Configuration with Roles]$ cat tmp

Looking back at our environment for calls that we have three tiers in our application and each tier

is configured by a separate playbook.

So inside of each playbook for each tier we have a number of tasks that are really configuring that

tier specifically for our demo application.

Now let's say we had another team that came with their application and they wanted to use Ansible to

manage their applications.

So consider this question How much of our existing code could we reuse for that new team.

Now you could copy all of your playbooks and paste them and then go through and ended up everyplace

we put in demo and replace that with the new application name.

But that's kind of tedious especially if you think about more applications coming down the line.

Also what if that team has a slightly different configuration that they need or they want to listen

on a different course.

You could edit those values.

But again now we have some divergence between all of these playbooks that you're copying.

And ultimately this is a problem let's say if you come back and find that you need to make a change

to your database configuration.

Now you've got two or three or many different playbooks that all require that same update lot of code

duplication.

Ideally what we want is to be able to have a single place that we can figure out why my SQL Server would

look like and then an easy way to instantiate that for each app to customize it for each app without

having to do a lot of code duplication.

But ansible gives us to solve this problem is rules.

So ansible role is a folder or we can put all of the tasks files variables handlers that are a part

of that specific function.

And now it's encapsulated in this one space and then we can compose our servers as a combination of

various roles rather than have all of that configuration mashed up together kind of in our root.

So we get two benefits out of roles.

One is the coterie use which we talked about.

You can then use this rote for multiple applications.

But even if you're not planning on using multiple apps even if this is all you have is this one app

.

It still makes sense to start moving towards roles because of the encapsulation.

It's a good practice because as your services scale out as you manage your Aspel code keeping everything

in the root means that you know templates directory that recreated that files directly recreated is

going to grow and it's going to be mashed up with all the different roles and being able to separate

those out might be difficult over time.

So moving to roles is a good way to set a foundation for scaling out of a structure and knowing if I

need to touch something that's nginx related.

I know I can just go and look in the nginx role ansible has a public repository for roles where you

can look and see how other people have and implemented some of these functions.

And it's called ansible galaxy or we'll cover that more in-depth in a separate lecture.

But we will use the ansible Galaxie tool right now to initialize it and scaffold out our role structure

.

So we're going to jump to the control host here inside of our Aspel directory.

I'm going to create a Rolls folder and CD into that and we can use the ansible galaxy command line tool

with in it.

And this is going to create a skeleton for us for our role.

So let's take a look at what this looks like and set of rolls.

Now we have control and lots of folders here and the names of these folders look should look pretty

familiar.

So we've got files handlers tasks templates all of these things we've we've configured inside of our

playbooks But now the role structure gives us a place to put all those things in a very structured format

.

So for instance inside of the tasks we now have in mind.

If you look inside of it it's an empty XML file.

But here's where we would put the task that we used to configure inside the playbook for this particular

role.

As we continue we'll cover all of these in depth and work through the migration but this is just to

show you kind of what what the road looks like before we continue onto the next topic.

I'm going to go ahead and scaffold out the role structure for the rest of the roles we're going to use

.

So when you create one for nginx

Apache to one specifically for our demo application called demo app.

And finally one from my school and notice that I'm making these specific around the actual application

the function that it's serving.

So in the case of load balancer it's going to use nginx kind of a one to one mapping.


But then also our demo apps there's really two separate roles that we're combining into the web server

function.

And again my Seacole one role for the one service with that up next we're going to look at migrating

our playbox into this new role structure

All right let's get started migrating our playbooks into our new role structure that we've created and

burning a play book into a role is relatively relatively straightforward.

It mostly involves just moving the contents of the files that we created in the play books into their

new homes side of the road directory.

Let's start with control here inside of the control roles.

We see the tasks mean I am going to open that file up and also look at the control YML file.

So here this is very straightforward.

We just have this basically this one task.

So what we're going to do is copy this task here and place it inside of our main dynamic in the control

room and that's it.

So notice a few things.

One is we didn't take the tasks with us because this is the tasks main dynamic.

It's assumed that everything inside this file is a task.

So just top level is the list of tasks that we want to execute and we can take that just right out of

our tasks here.

So now that we've done that the content is moved over we can replace our task lists with the roll inclusion

.

So we'll have a Rolls key and we'll add control just like that.

So now what we've done is instead of saying when I execute this control playbook we want a number of

tasks to execute.

We're saying I want you to go and find a can the roll call control and execute whatever tasks it has

for you and it will go through very similarly and execute step by step whatever tasks are in place.

Same thing.

So functionally no different but we've moved location of those tasks over into the role and show you

the free execute control YML.

We should see that no no real changes have occurred because all we've done is move to move the location

of the tasks.

Functionally this service is in the same state and we're asking to get into the same state.

One slight difference in the output here we now get to see the role and the task name.

OK so everything's OK.

No change.

That's what we'd expect.

Let's let's take care of the database as well.

So in the my school directory open up the pass folder here.

So database again we've got a number of tasks we're just going to grab these tasks up and down in our

database file and save.

And as we're going through this there's one thing that I notice that I want to address.

So here we have our my school started service line and then our configuration line here.

Now this is something you may run into already in which case you may have had to monkey or had it work

around us to get to get back up and running.

We're going to talk some more about ordering problems inside of the troubleshooting section.

In this situation what we have here is we were forcing this to get up and running.

Now if something were to fail during let's say this line in the configuration the first time you we're

up and running everything's good.

We go to the configuration line.

So we put in a bad value everything restarts because of the handler it fails but we but the playbook

completes the next time we go through when it attempts to start it.

It's going to fail because we can't start because of a bad config value and Intel is going to exit the

playbook so we'll never get back to the place where we get to update it and fix that typo we made or

that her that we had we're basically deadlocked.

So again we're going to talk about some other strategies for how to fix this.

One simple way to do it and what kind of what makes a lot of sense of rethinking through what we need

to do is let's move this location of where it started.

After all the configuration items.

So this is something to look out for.

It's a kind of in-place refactor.

It's something that's easy to miss.

Come back to bite you later.

But by doing this we now say we always want to give this service a chance to reconfigure itself before

we get it up and running and re-enabled.

So let's make that change now.

The other thing that's going on in the database playbook are the handlers.

So similar to the tasks we just grab the handler and instead of putting in tasks main put it in handlers

main right there.

And what the role is giving you is the folders in the files of where to place them but also ansible

Matthaus the awareness of how to go and look for these.

Look for these specific tasks so you can use the exact same way inside of our main door that the Notify

doesn't need to change.

It's going to work exactly the same way.

It knows that the handlers are in the handlers main dot yml for this role.

So with that done we can replace both these tasks and handlers key again with a Rolls key and will fit

in the sequel roll right there.

Right that's two down two to go.

Up next we're going to look at our nginx and Apache rolls to configure our web server and load balance

or peers

With our database and control tiers migrated Let's move onto our last two tiers the load balancer and

the web server.

In addition to the tasks and handlers these tears are going to require us to move files templates as

well.

So let's take a look at that.

We'll start with the load balancing.

Opening up the load balancer here.

We have a number of tasks to move when I pull those out and grab our engine ex tasks main YML.

And it is going to pay season and similar to what we did with the bicycle so we're going to go ahead

and move our service started line down to the bottom of the file

back and load bouncer.

Am I going to grab our hndler place and our next handler's file

place or cast key with a Rolls key to pull an engine it's

now ending next.

Also has the template right.

So let's look in the templates folder.


It's like the folders with this thing.

It's great that

we go.

And with moving that template we mean we need to make one one small edit here.

So inside of our main dining I'm all for the engine next roll it's you the source location for the templates

is in the templates folder.

That's where it was relative to the playbook before inside of a roll.

Templates are always assumed to be in the templates folder so we can just delete this part out and it's


And let's look at the let's look at the web server here.

So inside of web server now we're going to be splitting this roll into two different.

We're going to put this tier into two different roles.

So it's going to be the Apache tier which is sort of the base generic Apache set up the stuff that you

would use for any purpose any web application running behind Apache and we're going to have the demo

at civic stuff.

So looking at this list we have a few things in this web could cons list that's relevant to demo app

and some that are relevant to our Apache service.

So I'm going to copy this and paste it into both.

So in the case of our Apache service we can get rid of these python dependencies

and I'll go and drops that into our Dymov as well leading out the Apache dependencies.

OK so back in web server YML there is that now.

OK.

So all of our Apache lines will take these here

along with deactivating the default

and we need the Apache to notify the handler here.

So we're in a grab a handler and move it into the Apache to roll

it and the rest of this content

will go into our demo out here

.

And because we're also going to be restarting Apache in the demo app role we need to know there as well

.

OK in this case our web server now is going to be replaced by two roles the Apache to roll and the demo

after all.

So in the case of our web server we don't have any templates but we do have all of those files right

our Python application files that are in that directory.

So I am going to move

all of the demo directory into demo app files just like that

.

One last change we want to make similar to what we've done with my simple nginx.

Let's move our Apache to service start to the end right there.

All right.

So we've completed basically a direct port from our playbooks to our roles.

We have our on Capitalization that we want.

However we still don't have a lot of reuse of modularity because we still have all those demo specific

hardcoded values sprinkled throughout all of our rules.

So we're going to try and tackle that next before we go and do that though.

It's a good it's a good idea to stop and make sure we haven't broken anything or introduced any regressions

in the next lecture.

Well we'll run a re-application of our roles and do a quick status check before we move on.

All right we've moved all of our content out of those playbooks in the root and replaced all of those

with roles.

Before we go on.

It's a good idea to stop and just make sure that the direct translation hasn't actually broken anything

.

And to make sure we didn't miss any tasks or we didn't copy something incorrectly.

So we're going to run all of our playbooks again and then run our stack status to make sure everything

is still in place now in the past what we've done is just run each individual and your playbook.

So one for database one full accounts or one for web server.

We could do that again.

But instead there's an easier way that we can collect a bunch of entry points together.

Basically a playbook that contains other placements.

Let me show you how to do that.

So here I'm going to create a new file

I'm going to call it site thought YML psycho analyst sort of the traditional name for a site wide playbook

that configures everything you need in your entire stack and inside of this rather than repeating all

of the hosts to the rolls logic that we use in the individual playbooks we can simply include them.

So by typing include control like amyl it basically takes all the contents of control like YML and runs

it right here.

So include is a multipurpose action you can use it to include a playbook inside of another playbook

like what we've done here.

You can also use it underneath the tasks key if you have a separate file that just has a list of tasks

.

Kind of like what we have in our role with the main channel that's just a list of tasks you can include

that under the tasks key you can use it inside the handlers key and it is kind of a multi-purpose function

you can use in different places.

It's a great way to reuse code without building out a full fledged role let's say if you have some tasks

or handlers you want to reference in lots of different places.

So in addition to control YML let's let's go out and create one for each of our tiers.

I'm going to build it from the bottom up here database web server and load balance.

Right so there's Arcite YML.

Let's go ahead and run it

.

All right so that's completed.

And we noticed that everything is green.

Everything is OK.

Not a single thing changed and that's really kind of what we'd expect for an in place refactor where

we've just moved the ansible code around but haven't changed anything on the state and the systems.

One last check to be thorough.

Let's run our stock status playbook.

Make sure that everything is still up and running as we expect.

Awesome.

Everything's in good shape.

Let's continue with our refactoring

Know we've got our new role structure in place and we're fairly confident that we've moved everything

over without introducing any regressions or or new mistakes.

Let's take a look at doing some some real refactoring in earnest.

So we've got our application working remember.

But part of this the goal in this section is to go back and look for places where we've hardcoded values

or made some assumptions about our application and extract those out and replace them with variables

of parameters that we can override in case in a different environment or a different deployment.

We need to tweak something we could do that easily without having to go in and manually write something

in the playbook for inside of the road in this case.

For starters let's look at my simple service.

So I mean to look at my simple roll tasks I mean YML.


from our web server tier now.

In our case we only had one port on this instance.


We ended up just listening on our main interface.

But suppose that we don't want to have this policy where any interface gets it's available from a single

connection.

We want to lock it down to just specifically the SERO interface.

So here's the problem.

At the time that we're writing our code we don't know what that eats or IP address is.

So we could log into the server if config look on it is come back and hard code that in but that doesn't

really help us or move us forward.

It's actually going to make it more brittle and not more robust in the end which is what our goal is

.

So instead what we can use is a dynamic variable that answer them provides us and that's a fact.

So you seen when we execute playbooks that one of the very first steps is gathering facts.

And what that's doing is going into the host and getting a lot of relevant information about the host

and bringing it back so that we can use it for purposes just like this.

There is there is a way that you can query and see what all the facts are.

The answer is going to collect and that's using the setup Marchal.

So if I type Ansible dash setup let's hit the DVOA one host for instance.

So this is going to basically just go out and execute the equivalent of the gathering facts step.

So we've got lots of good information here and all these variables start with and underscore some some

information right.

And all of these are generally going to be available depending on what the host type.

But since we're all using a going to Linux here we expect to have all of these facts available for each

host that we connect to and you can see here that there is an answer bill is zero.

Before I address facts that we can use to get the IP address and the great thing about the fact is it's

relevant to the specific host at a specific time that we run it.

So if any of this stuff changes in the future the gathering facts step is going to grab the new version

and allow us to date it.

Of course normally run through and we don't even have to be aware of that fact has changed.

And Apple's going to handle that for us underneath the covers inside of our playbook.



that that hash that address.

So now it's going to happen is when Ansible runs it's going to do the gathering facts to find out the

current address for that interface and when it goes to update the binder address it's going to inject


And that way we can be sure that no matter what happens my sequel always only ever be listening on the

east sirra address

so we can save that and we can run it

back over here.

Let's run our playbook.

This is going to be our database.

All right.

And that's complete.

So that's a test to make sure everything is still good let's run our stock status playbook.

And here we have an issue when we get to our listener so we notice here that our stock status.

Wait for line by default is looking to see that it's listening on our local post.


But now we've locked down and tightened our configuration even validated our support plate playbook

.

It's really important that we always go back and make sure that our support playbooks are lockstep with

our with our configuration playbooks.

If we take care of them during this time that we're developing and testing and iterating they will take

care of us when it comes time to a production issue.

We're troubleshooting something live in production.

So let's go ahead and take the time to go back and edit our two playbooks stack status here at this

line.

What we can do is add a host Fran.

We're going to use that same ansible Ciro IPV for a dress and this is going to say you know Walmer checking

our database hosts we want to make sure that it's listening on that exact same address that we expect

to configure we're to make to change here

.

And we're also going to go to our restart playbook which is doing something very similar and the same

logic here.

Now we run our stock status.

Again we'll check to see if this is properly checking their status on the port that we're listening

on.

All right.

Everything looks OK.

Up next we'll continue refactoring Our My sequel role

Let's

continue refactoring or my sequel roll.

So looking through the task list we see here at the bottom that we've hardcoded in the demo database

name user password and host source host for that user connection into this role.

And our goal is to get this my sequel were all more modular so we can use it in different apps.

So in that case it doesn't really make sense to use a demo across the board the way that we resolve

this really depends on how we plan to use or apply this role in the future.

So stepping back a bit in our case we're assuming that my sequel server this role is always going to

be used for one and only one that is the server is no good to us if it doesn't have a database configured

so we can always assume that's that some database user is going to be created at the same time.

We're not going to support multiple databases multiple users inside of this this role so this role is

really set up for inside of some application cluster.

We're going to spin up this instance or container or however it's how it's used and it will support

the app that's inside of the same cluster.

So in that case we can assume that.

My sequel DB and my school use are lines will exist and only will exist once safely.

We just need to to make this more generic and remove the references to demo.

If you expected a one to many relationship or a multiple number of databases you could actually get

a few different ways you could use a different role for the database creation something like a database

consumer role.

You could also use something like a with item style loop and pass multiple database parameters.

We look at how to do something like that in the future in our case we're going to keep it really simple

.

One database one user all the time.

So let's start by removing references to the demo setup and I'm going to go ahead and plug in some some

default variable names that I think makes sense.

So in this case I'm going to replace the demo with DB underscore name

here at the name of the user over place with Eby underscore user name

passwords.

Very similar DB user pass and the Priv.

Here there's a demo refers to the database name.

So I use that same DB underscore name.

And so this line is going to kind of long.

They put this down here on this next line and host here rather than assume that every user is going

to connect from any source host.

Let's allow the role to take this parameters DV user host allow the consumer to specify I need to be

able to connect from this single source stream.

So save that.

So now we're expecting four different variables to be passed into this role.

So if we ran the database Tamela at this point it would fail because DB name and user name and these

other parameters are not specified anywhere and they're required to finish these task.

So that would be a problem since our goal is to make this my sequel roll kind of standalone.

We can provide it with some default values that allow someone to instantiate this role and bring it

up even if they don't have particular values that they care about.

So for instance if you just want to spin it up and have a bicycle server for a throw away or for a generic

app you don't have any special configuration.

We can populate and defaults to allow that particular case to function even when no specific variables

are provided for the overrides.

So to do that we're going to use the defaults file.

So here in my sequel defaults main yml will go and provide some sane defaults so DB name is our first

and I'm going to give it my hat.

So you can call it whatever you like.

In my case I'm saying if you don't care about the name of your database inside of this server or just

send it call it like.

Similarly for DB username you can say it's DB user the password is going to be DB pass and our user

host is going to be localized.

So now we can simply include the my support role and we'll get a functioning Server database.

Yours are all ready to go.

You could also get the default altogether and what would happen then is if you know that you're only

going to be using this role in sort of production like way or or for a full stack database for this

app if you leave the defaults out when someone tries to run it it will fail and tell them this or does

not.

Is not defined and sort of forces people to define it before they'll actually get it to execute all

the way.

And then you don't have a halfway configured database or incorrectly configured database in your environment

.

All right.

So now that we have our my SQL database with the defaults we could go ahead and run this and apply this

as is.

However if we did that we would end up creating this unnecessary map database with this new user.

So let's not do that in the next lecture I'm going to show you how to override these defaults and replace

it with the actual values that we already have deployed.

So we'll continue running in our stack without introducing this unnecessary database.

Let's do that next.

We swapped out our hard coded values inside of my school roll for some sane defaults how do we go about

overriding those defaults with variables specific to our app.

That's all we're going to take a look at now.

And pulling up the documentation here.

Foxton ansible dot com and in variable's section here under playbox there is a note here.

Verbal precedents.

Where should I put it.

Now let me warn you in advanced verbal precedents ansible can be a little tricky and a little overwhelming

as it's not always intuitive.

It's also changing a little bit between one dot version one and version two or at least it's getting

more explicit and getting clarified.

So it's a little bit tricky.

I just want to note a few things and help you build some intuition around how the variables work.

So if you look at this list the one that ex-president's which is the version we're using right here

is this roughly.

This role defaults go first and then fax and then inventory everything else which is a very broad category

and notices it's everything else is in quotes connection VARs and extroverts.

So what does that actually mean.

So notice that the first lowest priority the first thing on the list is roll defaults and that's where

we just put our variables and it's precisely because they're the lowest priority that we put them there

.

We're saying if nothing else is specified you don't get.

You don't get values for these variables anywhere else.

Use these.

So they the lowest priority or the safest the highest priority down here is using dash or extra vars

.

So at the time that you actually execute ansible if you type ansible playbook Dashti and then give a

key value pair.

There you can override a variable and it will be used with the highest priority.

You can be sure that any time that variable is used it's going to have that value.

Ideally you're not going to be passing those variables in via command line as a regular course of business

because doing that you expose yourself to maybe forgetting it when you need to run at some time if it's

actually critical.

Maybe you have a typo.

We're trying to get away from lots of manual entries so trust know that it's there you can use it as

a last resort or for some special tricks if you need to overwrite something.

But for the most part you shouldn't be injecting regular variable values in via Dashti.

And between those two extremes there are lots of options that general prep the general rule is as you

get more explicit in scope as soon as you get closer to the actual task definition execution layer the

higher the precedence gets.

So you can remember that still it's not always clear.

Ansible allows you to append to Varas key at very lots of different places throughout the configuration


Let me just show you briefly what some of those things look like for you good.

So you get a sense of here are the places you can inject some vars if you need to.

OK so we have insight of the role.

You'll notice that there is a VAR's file here.

So here's one place for my sequel you could put a specific Farr's sort of the downside of using this

it has a high priority but the downside is you don't have to go into your roles and inject variables

for for different roles inside of those those those particular directories.

And you really want our our roles to be reusable.

So putting specific variables to our environment inside of the role out why it works.

You can do it.

It's a bit of an anti-pattern because it's going to be harder to understand where exactly are the specific

variables that I need to use for this particular instantiation.


and provide a key value set to YML dictionary hash of the variables and they'll use them for that specific

task.

You can also include vars Pat.

Let's say the the play level.

So if we go back here for instance we could pass fours like that and put the key value list and they

would be used for everything inside of that play.

You can also pass vars inside of includes And inside of rolls.

So if here are saying include control the answer will gives us the ability to pass key value pairs here

and override as well as inside let's say you know back here control we can pass keys in here and test

the values.

So that's a lot of information.

I know.

And the reason that I'm going through all of this is to just show you prove to you you can put the vars

kind of wherever you need to.

But I'm going to make recommendation rather than sprinkle your VARs and all these different places.

Pick a simple scheme kind of pick a simple place where you want to instantiate your bars and just stick

with that.

It's going to make it a lot easier in the future to go back and answer the question.

Where did I find this variable.

Like where do I go to add it to scribble value.

You want to use a very flat hierarchy just from my experiences.

What we found flat is better simpler is better.

Yeah and something else to note is that ansible scoping is global in nature.

So whenever you define something as a VAR it enters a global scope as a common namespace in intervals

.

Doesn't give you much ability to define smaller scopes.

You can use objects or hashes to build kind of a hierarchy inside of that.

But just remember as you're defining vars even if you're sprinkling them here at the task level or at

the at play level they're all entering the global scope.

And so it's it's it's hard to keep track of those variables if you don't have a good system a good pattern

.

And that's why I recommend HC as a single place or one or two places where you know I'm going to put

my vars here and you have a sort of single source of truth that you could then go back at it and trust

that I have predictable ways of managing where my variables are defined.

And then you know this is the value that will be used with very little hesitation.

So in this course I'm going to aim for using three levels at the most.

One is the defaults inside of the roles for pure defaults.

And in some ways that's not even really a virus declaration.

Those come with the rolls.

So you kind of get them for free.

But I'm going to start putting some values there.

So those are the defaults.

The second layer is on the roll inclusions.

When we go to instantiate a roll we're basically going to pass and some of the values that we expect

that rolls to take as parameters.

And so you can see right there I predict this role and I'm using these specific values to hydrate that

roll with this with this custom information this data and then the last is to use a separate file.

And that file can be included via a virus file.

My recommendation is to use a Group group vars file.

And really what rebars is saying is it gets picked up automatically ansible allows you to specify a

file that matches the group name.

So I'm going to use group for as all you don't have to specify at the top of each roll.

And then when you run you know that all of these kind of global application specific values will be

injected and used and Raptr application regardless of which playbook or roll it's being it's being used

.

So it's a lot of information we're going to talk about roles and variables some more as we go on.

But if you take away any one thing from this lecture it's Keep your very simple.

So for our my sequel role we've created those defaults that you remember.

So what we can do then is here as we instantiate the role we can change from a simple string inside

of this Roki and instead replace it with a dictionary or a hash that has the variables that we want

to inject.

So in our case our variable DB name is not my app as we want it to be as it's configured by default

.

We want it to be demo which is what we've used before.

Our DB username is also demo Eby user pass also demo.

And finally our Eby user host is going to be the percent sign in our case.

We've already done this.

We know that our set up is configured safely so we're going to use set side to allow us to connect easily

from those Web servers back to them.

OK.

So now that role is being included in unpopulated.

Feel free to run his playbook again and run stack status.

You want to verify and if you do that you should see that no changes occur just as we as

All right as far as variables go are my school Apache to control role should all be squared away.

Let's take a look at the engine load balance rule similar to our my school server.

We can make some mashup assumptions about the way that nginx is going to be configured whether it's

a one to one relationship between an nginx front end and a web server back and not necessarily one to

one number and Web servers in the backend but just will bring up a new load bouncer for every web server

stack we have.

We could do that very similar to what we do as my school to keep things interesting and and show you

a new concept.

Let's assume that the engine ex-service may need to load balance multiple servers and when my servers

I mean the internal concept of nginx we are listening on the front end for multi-sports and for each

Front-End for we'll have a number of back ends or reports.

So this in the real world this might be our application server listens on one port for our user web

traffic and maybe has an additional port for our rest API traffic or something like that.

So how one nginx instance upfront but it has a few front facing ports that's listening on in the past

we've used with items as a way to pass multiple arguments and to do a task and execute multiple times

.

Let me show you a variation of this pattern using with DECT and Dick in this case is the CTE dictionary

or hash.

Or you could think of it as an object that has NO NO functions just variables.

It's just a key value mapping together.

If you're not familiar with it.

So in this case let's start with setting up the defaults first.

I'm going to go to defaults main YML and we're going to set it up like this.

So at the top level we'll have a site's key and we'll use this as a signifier to say everything inside

of this dictionary is a mapping of some front end app to some back and forth.

So the default is going to be again we'll call in my app if nothing is supplied and it will specify

two keys inside of it that's the front end port and the back and forth.

So because this is a dictionary hash if we had another we could say my app to pass different front end

ports and back and ports and that would allow us to then theoretically configure multiple pairs of front

and back and listeners on this in the next instance by default.

We're going to stick with the basic single front end single backend setup similar to what we have in

ours and in our environment.

So it's going to save that they said Now we need to go and update our tasks or templates to use this

new ways to structure inside of main dining YML.

We have a few places where we need to weed out our demo hard values.

So the first is in configuring the nginx site.

So here we're going to use our new key with dict and we're going to pass sites which is that that we

supply in the defaults.

OK so now similar to with items where we need to use the item key inside of our actual task.

With Dick is going to give us something very similar instead of just plain item.

We need to use item Docky because the dictionary the hash is a q value mapping we can refer to the key

as item dot key.

Looking back at our default here sites is the top level hash.

The key will be my app and the value will be this this embedded dictionary up front end back and mappings

.

So there is the item Dockerty.

We also need to do this with stick sites down here when we decide to activate.

OK.

Again we want to remove the reference to demo only replace that with item key as well as this one over

here.

Items like this.

And let's also remove reference to demo here and the names of the of the tenants.

So that's the task list.

Remember we have a template as well we need to update.

Let's take a look at what that looks like.

And here we have the upstream demo which again we can replace with item and key.

And this may feel a little bit strange that we're using the item key inside of a template.

Now inside this loop when we go to build this task we're going to be kind of inside of the with Dick

Lugar So the item variable is going to be populated with the current index basically or the current

heat of looping through.

So we can refer to item inside of our template to get the right values out at the time that it's rendered

.

So here we have Ivan Doxey for the name.

And again down here I'd item Docky.

Now here we're going to use the other values that we decided to inject into our into our hash.


value the front end that's going to be whatever Portelli we specify.


to make it explicit so that it's passed in.

So we'll have we'll pass a colon and item dot value dot backend.

So this is the back end for whatever we're listening on from the web server side we have the front end

port.

And with that we have everything rolled over to the point where we can configure the full stack on nginx

instead of just being hardcoded to expect a single front end back and where we can loop our dictionary

and get multiples if we desire.

So now that that's updated let's give it a go.

We're going to run ansible playbook load balancer YML.

So we didn't supply any overwrites here.

So what's going to happen is since we're using the defaults our nginx instance is going to be created

with my app as the name of the upstream internally.

Now from our perspective the backend web server the user will never see that label.

So it really doesn't matter.

And it should run through and configure everything with this new label but with the default back and

front end pairing of ADHD since that's what we're using anyway.

We expect this to run through an ad and get up and running with no problems and everything appears to

be OK.

Let's run a stock status playbook as well

.

And there we go.

Everything is up.

Everything appears to be okay.

But an astute observer wouldn't notice that we've injected a small problem into our environment and

that's something may come back to bite us in the future if we don't care.

Now see if you can figure out what that problem might be and in the next lecture will jump in here.

And The Last Lecture we used our updated nginx role with just the defaults and applied it to our look

balancer or we saw that everything reno went OK.

But I mentioned at the end of the lecture that we had just introduced sort of a hidden problem.

Let's take a look at what that is.

Now from the control machine if I log into our load balancer host and look at the contents of Etsy and

Gen-X sites and enables we see that there are now two active sites.

Let's think about how this happened.

So first we had our demo site that we had configured with our static hardcoded values.

We then replaced it during our refactoring.

With this my app that's powered by our default structure and when ansible ran it no longer knew about

demo it saw that it needed to configure my app which didn't exist.

So it put the my app site in place it activated it moved on everything restarted everything looked great

.

So the reason that this happened is because once demo fell out of the playbook ansible stopped caring

about its existence.

It's still there on the system.

It's Third it causes problems.

But Ansible is only going to worry about what you worry about.

Actively what's in the playbook right now.

So a demo is there.

And because of our risk factor we now have this potential conflict.

So there's a few things we can get for this one is even when we're using configuration management to

manage our infrastructure.

Drift happens.

It's just a fact of life and we're managing our systems this way unless you're regularly resetting your

systems or using something like an immutable infrastructure deployment scheme.

We're going to have drift that occurs.

So these automation systems are not foolproof.

You just need to keep that in mind and look out for it especially in this debt cycle.

The second thought is if you need total control over the folder or configuration set you need to make

that explicit.

So in this case what we need to do is tell ansible not just to create a site that we want but make sure

that there are no other sites other than the ones that we want.

So to address this issue we're going to actively scrub out all of the sites that we're not interested

in just in case something else has been reactivated or snuck in in the mind set to do that.

We're going to mash up a few of the tools we've learned about already register with items and when we're

going to use that to lead out for unnecessary sites.

So inside of our next role we're going to go to the tasks.

And right here before we deactivate our default engine next site for an at a new task and call it get

active sites.

So what we want to do is before we before we go through and delete out all the ones we don't need let's

get a sense of what's already out there and configured because we may not know in the case and in this

case your demo is already out there.

We didn't know about Aspel had forgotten about it.

Let's get a list of what's already out there.

Get active sites.

We're going to use the shell Monsell which is very similar to command but it just gives you a full bash

shell instead of a subprocess command.

And we're going to execute.

LS one just gives us one out one entry per line at sea.

NginX sites enabled and we want to register that into a new variable called active.

So here's what we've done.

We've said Let's check to see what's out there and activate it.

We'll get a list back and we'll register that in this active variable.

Now in our next step in the default step instead of just deactivating the default site we can deactivate

all of the sites that we don't want and using the contents of that variable that we just ran.

So we have our file line is going to remove status.

Absent any of these links that we don't want we're going to check with items active standard out lines

and what this is the active is the result of that last task.

And that result has lots of different variables the return code to standard out the standard error lots

of attributes we are interested in the actual line by line standard count.

But that last command gave us so we're going to look through each of those and for each one we're going

to use a when clause when the item is not in the site's key.

We want to then deactivate the item.

So what we're doing we check to see what's out there.

We look through each one.

If it's not configured in our site's key which is recall what we eat what we can figure and defaults

and what we'll use to override in our sites if it's not in there which means it's something we want

let's get rid of it.

So this now two step process ensures that we're going to scrub out any sites that are not part of our

sites.

So let's drop out of here with this say.

I'm going to run ansible playbook a bouncer

.

And here we see that we got our active sites list back in the case of demo.

This changed means because this task is actually doing a removal.

Changed means we found item in demo.

We don't want it let's get rid of it.

My app is inside of our sites so that when clause will return false.

In that case we skip.

So that will preserve my app and remove demo and now we have a single site we can sleep a little more

soundly tonight.

We can continue on with our refactoring

All right we have one role left to tackle as a part of our variable refactoring work and that's the

demo app role.

Now since demo app is really purpose built for its own application we're not really concerned about

the modular reuse of the app but we do want the encapsulation so that we can keep all of the all of

that data contained inside of that.

And what we also want to parameterize the interfaces that demo app uses to connect to other roles that

say so if there are connections into the Secret Service or to an engine ex-service we want to get those

parameterize so we can easily edit them so that it can connect to the right dependencies.

And another use of using parameters inside of roles is really if you want to extract repeated data points

so for instance we have all of our files in var dub dub dub demo right now and that's sprinkled throughout

the entire roll.

If we wanted to change that to use or shared demo for instance for some reason in the future right now

we'd have to go into lots of different places and edit it.

What we could do is create a variable called Path or something like that we could default it to our

demo and then if we wanted to change it we could easily just go back and have one place to do the update

.

So it's a little bit of just variable management and reducing duplication.

I'm not going to do that in this course because in this case we need to change it a while well-placed

said one liner can take care of that for us.

We don't have to do all that upfront work just in case.

Which brings up a point you know it's easy to go overboard.

You don't need to use all the features of ansible in every role.

Keep it simple.

Tackle the things that are giving you pain but it's alright to leave some of this stuff in place and

get on with making progress.

So we're just going to continue with our role and look at something out of my Stiefel parameters inside

of our demo so to do that we need to take a hard demo Wisky file which is where all of our database

parameters are found.

So here it's an demo app demo whiskey and this username password DB name are all here.

Now if you want to parameterize it we need to take it out of files and put it into templates so that

we create that directory here since it's been removed and spool rolls out

.

And I'm just going to drag this file into it and it's directory area.

So now we can just simply sub out the variables in the template with the variable names so demo here

.

I'm going to call the user.

This is our DB pass rather than hard code in our database hostname.

We can use our groups dot database key like normal and we want to connect all of them because that's

a list we can just pull out.

Let's say the first which we always assume is the master.

In our case there's only one and then here is the VB name.

All right.

Now small change is where the templates directory.

Let's rename this to demo Wisky that J to signify that this is a template

and because we now have a template that's not inside of our files that are getting copied in bulk we

need to go to our tasks and create a new template line here.

So

right after copying the full source contents for demo app we're going to change this to a template.

And when you make this copy and this is so the source of that was the job just to remember because it

is a template is going to automatically look into templates directory and our location is part of the

demo.

And then let's give it the full path to the file with the correct extension and.

So in this case we could add a default for these values but I'm not going to do that.

And because just if you think a step back and think about it this demo app is only going to function

if it's connecting to a real database out there somewhere.

So in this case to me it doesn't make as much sense to add a default with some dummy database values

.

We really need it to connect to a real database.

And in this case by not specifying a defaults if we forget to provide the real credentials animals going

to yell at us and tell us that those values are missing which is what we want.

We want that failure to tell us we've missed a value and rather than just running silently and configuring

something that's incorrect.

Also notice that inside of our file I chose names that were different than the ones that we used in

my sequel roll.

Since we're configuring both roles ourselves it would be convenient to name them all the same thing

.

And that would reduce some of the headache.

But realistically a lot of times you're not going to get to control all the variable names especially

if that role is something off the internet or didn't have or if it's maintained by somebody else on

your team or a different team.

So we're going to have to tackle a situation where we're going to we're going to call the variable one

name ourselves and somebody else is going to call it something different.

But the underlying data value is the same value.

So how do you tackle that.

We're going to we're going to address that here coming up.

So just like the my simple role though if we were to run this now we would have the problem like I mentioned

we can inject the right values that we want at the time that we create the role.

So if we go to web server here demo app we can change this to role.

And the app and then populate the values that we want.

The name is demo.

The user is a demo.

He passes a demo.

All very unimaginative.

But it's going to get the job done.

OK so now that we have that let's give it a whirl and we'll playbook web server.

All right and everything is completely green.

Nothing changed.

Which means we had a cleaner factor in place.

We are in good shape.

Let's continue with the next lecture.

In the last lecture we set up our demo app using the database credentials passed and the vars for the

role.

And remember that we also did the same thing for my simple role passing in those database credentials

that we wanted to configure at the time that we defined the role.

So now we have the database name user and password defined twice and that means if we want to change

them let's say swap the password we have to go to two different places and hit them.

Ideally we would have a single place where any particular data value was configured.

And just updating that one place would then cascade out in any place we needed that variable applied

.

It happened automatically.

Remember too that the names of those values were a little bit different between my sequel role and a

hard demo app role.

So one case we had DB underscore user underscore pass and in other place we have DB underscore pass

.

So if we were to just put both of those into a single source of truth we'd have to duplicate it unless

we had some way that we could configure a variable once and then use it under a different name twice

and we do.

So we'll look at how to do that as well.

So in order to accomplish this we're going to extract out that single source of truth for variables

into a separate vars file.

So ansible gives us a few different ways that we can keep an external variable finals.

One is the inventory which we showed you way back in the beginning.

In this case we're not going to do that because we want to keep an inventory kind of compact and just

with the list of hosts and separate variable logic that's really dependent on our app that's not tied

to any particular host from the list of hosts that we're using inside of our cluster.

So we're going to skip inventory for that option.

The second option is a Varas file which is basically just a YML file with key value pairs.

That's a good option.

There's lots of good uses there but it requires you to include that vars file explicitly and anywhere

that you want to use those variables.

Although that's handy it's a little bit a little bit more overhead.

The last option is to use the group far as an host for structure.

The nice thing about group far as is if you put it in the right place it automatically gets picked up

and you can segment by group.

We're going to use group far as Paul so that all of our playbooks all of our roles will benefit from

it and we don't have to specify use this file every time we run a play book.

So it's it's sort of seamless and it gives us that place where we can specify variables and they get

the market picked up.

So what we're gonna do is create a directory here inside of our ansible root called Root farz and inside

of that we'll create a new file which will be a YML file and we're going to name it simply all yellow

.

And this will be a YML file.

So because we called it Group far as all all the different groups are going to benefit from these variables

you can create a separate file per group but rather than kind of break out and distribute all those

variables Let's keep them all together so there's just a single place we can go to look to update those

values and see what those values are.

So ignoring them I see people naming for a moment we're going to pick a naming scheme for variables

that kind of makes sense for our app domain.

And in this case it overlaps with the names that we chose for the demo app for that kind of obvious

reasons.

So we're going to put the values that we want here into this group far as all funnel the DB name is

demo the DV user is Demo DB pass again.

So now we've got our group as all file we've extracted out and we've said this is sort of the canonical

values that we want to use throughout our entire application.

So now in our database YML instead of configuring these here again we can refer to those values that

we put into group far as all.

So I'm going to do a little editing of the formatting.

As for personal preference here.

DP name I'm actually going to delete because DB name here is the same variable key as DB name at all

.

So because all of the variables inside of ansible are globally scoped same same namespace name is going

to filter all the way down get picked up by our my sequel role without having to explicitly specify

it here.

DB username we're going to tell it you want to use the value that's in DB user.

So you could you could think about this amain different ways Octa called variable routing but it's really

saying one variable is referring to the contents of another variable.

So we're putting this db user name VB user variable it's going to get subbed out and then when passed

to the roll the value will then be named DB username.

So this is this is how we conquer this problem of two variables named do different things referring

to the same Condon's below that DBI user pass.

We're going to do pretty much the exact same thing but in this case it's DB task that we're trying to

target and DB user host We're going to leave the same.

So for our web server definitions we've just put all these DB end user pass and name but these are exactly

the same values and names that we have in our group or at all.

So we basically is rendered this redundant.

So in this case I'm just going to revert right back to what we had before delete out all these extra

variables and just use them as a string.

Ok so now that that's done we have our separate gurdwaras file.

We can go back here and run ansible playbook database and I will we can check to make sure that all

those variables are getting populated and nothing's changing.

They're

all green and doing the same for the web server

.

Again all no changes.

So now we have a single place where we can put our files in if we wanted to change the password for

instance we could just change it here on our site YML and everything would update.

We can continue to add more variables to this file and they would be automatically available and all

of our roles and playbooks throughout our principal set up.

Up until this point though we've been using this plaintext password here in the past which is definitely

not a best practice and in the next lecture we're going to tackle how do we encrypt variable values

inside of an

We've been working our way through these roles with the goal of making them right.

So far I've been stressing modularity and encapsulation as the primary directives as we're doing this

refactoring.

Another big focus area lawyer going through this stage is looking at any of the security issues or security

concerns that might still be lingering from when we did our initial configuration.

One glaring problem here is that we're using a plain text password format for our my sequel database

user coupled with the fact that we're going to want to commit all of this code and data to a repository

so that we can keep track of it and birth control it and then probably build a deployable artifact.

We've got stage somewhere likely for a long time keeping that password in plaintext is really a concern

for us and we know we want to address that somehow.

Fortunately ansible has a tool to help us do that and we can use this tool which is ansible vault to

encrypt variables that we want to keep inside of our set up.

I've got the docs page here for ansible vault.

It's here in the playbook Special Topics.

Walt I encourage you to go here and look through the vault documentation to see all of the ways that

you can manage these files.

The big picture here is vault will allow you to encrypt the file with a passphrase.

And once you do that the entire file will be will be rendered will be encrypted so you can't view any

of the costs and something in plain text and you can commit that to a repots safely and then when you

run ansible you can supply that passphrase and ansible will decrease that on the fly and allow those

variables to be used unencrypted inside of the execution not to actually roll this out.

We're going to pare our group far as all file that we created with a separate vault fine.

So since vault encrypts the entire contents of the file it can be a little annoying to lose visibility

into what keys are encrypted in there.

And oftentimes this is for the sake of just a few variables so if you have one value that you want to

encrypt you're going to lose track of all those keys they're still going to be in there they're still

going to work but you don't have the ability to then grep out and say you know which keys do I have

and can easily look at those files.

So there is a best practice is kind of squirreled away in the documentation.

You can pair a vault file that's encrypted with a standard hunting trip corrupted vars file and then

refer between them to give you kind of the best of both worlds decryption for your for your secrets

and all of the other variables you can then use your standard tooling to look at those files and manage

them.

So inside of that retard's directory I'm going to rename the all file to vars.

I mean to create a new directory called Call and move our vars file into the all folder.

So here we're changing instead of having a file for the group.

All right have a folder a subdirectory inside of group parts for that group and then we can put multiple

files and all of those will be pulled.

And as part of that that group variables sort of vacuuming and at the time ansible pools and this is

data.

So I'm going to make one little amendment here that I can use Ansible volved inside of this VM and that's

going to export my editor I want.

Probably don't have to do that in your environment.

And then we use the ansible vault tool to create a new file seeing a CD into our old directory first

.

Now ansible vault create.

I'm going to create a new file and call it fault.

Now I'm going to ask me for a password and this is the passphrase that passphrase it's going to be used

to encrypt this particular file and I'm going to choose something that I will remember twice and now

I'm in this file and you notice that down here it's a temp file and it's because all of this content

is going to be put in plain text.

But before it's written down to the file system at the actual file that we want to use it's going to

be encrypted using this passphrase.

So inside of here let's add the value that you want encrypted and this is a standard standard that will

file as far as Bolt is concerned.

And I'm going to create a veritable vault DB pass and I'm going to give it some random string that I

created earlier here but I'm not going to use my secure password.

OK.

And I'm going to write it and safe.

Now we see that there is this vault file.


It's locked down.

Right.

So we've got this variable value that's a.

That's that's there we can't read it.

You can use ansible vault to take a peek at that and edited suffuse ansible vault edit the past and

passing your passphrase in you can then use your editor to pop back and do edits directly in place.

And when you exit it will then save again.

OK.

So I noticed that we appended the vault underscore name to that password and we did that because we

don't want collisions in our variable space because they're both getting loaded into the scope.

This part is strictly not required but this allows us to keep all of the keys in our vars file hunting

for it.

But then the actual values themselves save.

So here does Denbeaux.

I'm going to replace with a reference to our new encrypted key just like that.

OK.

So dropping back out into our ansible directory I'm going to try and run Aspel playbook site that YML

.

We'll see what happens.

And immediately we get an error message a vault password must be specified to decrypt.

So now that it sees there is a file knows that it's it's encrypted.

It's not going to progress unless we pass that passphrase to it.

Otherwise Ansible can do anything with that file.

Right so this is where the safe comes in.

So there are two ways to get around this.

One is you can use handsful playbook ask vault pass ask fault pass and you can pass that passphrase

right on the command line right before you get to the site on national declaration.

And it will then use that one time no password that you provided to unlock it and go.

So that's that's a very secure way because you're not writing it down to the file system anywhere but

retyping a long passphrase can get hold pretty quick when you're executing.

Over and over again.

So the other thing you can do is actually create a vault password file and put your passphrase in that

file.

And that's what we're going to do.

So instead of putting it here in the ansible directory though which is the place where we commit everything

to the repo I'm going to put it in my home directory and make sure that I don't commit it to the reader

.

So I'm going to put my passphrase here and on letting out the big secret it's mastering ansible.

I'm going to put it in vault.

That text in my home directory and I'm going to lock it down so that only the ansible user can read

it.

OK.

So now we've got some safety around our passphrase and we need to tell ansible about where to go and

find that back in the early early days of this course we use this ansible g file to configure a default

inventory's.

We didn't have to pass it each time we could we could use the Ansible playbook fault password file and

and pass the path to that vault past.

But again I don't want to make this overly verbose.

We have to type each time we can get away with putting it here and our defaults fault.

Password file is equal to home directory full passed up text.

And this way if long as we're executing from our ansible root directory it's going to pick up that vault

password file automatically and we're going to get we're going to get the ability to pull that in without

having to specify each time.

So that's saved and clear all this out and let's give this a world now

.

And notice that we didn't get prompted we also didn't get that error message so everything looks to

be OK in terms of that description.

But let's just let this run and see what it does

.

All right and let's follow that up with a quick status check make sure everything is still responding

now that we swapped out our database password.

This is a this would be an interesting place to check to make sure that we haven't lost connection to

our database.

And no so we're still connecting our database user updated fine and our our demo app was able to use

our new credentials that we passed to to connect to the database.

So everything looks good.

A few notes here before we finish off this lecture ansible vault only supports one vault password during

each execution.

So when you supply a password or password file it needs to be the password and the passphrase it's going

to unlock every vault file that it hits across that execution.

So normally that's not a problem if you control all of the configuration.

But if you have different teams contributing files that means you have to distribute that particular

vault password file to everybody who needs to encrypt data inside of the context of a single execution

.

So one last thing note the order of the execution site thought YML So we change the database first and

then we hop down and we change the web server next in a small environment.

This is not too much of a problem but from the time that we configure the database and swapped out the

user any new connections that still use the old value are going to fail.

If you've got lots of web servers that could be a long time before that last web server gets updated

.

So in a large number of posts or in a large environment it's probably worth taking the time to create

a special purpose playbook for doing a password rollout or a password rotation as opposed to using the

general site thought YML which is which is just kind of going through a generic order and it's up to

you to kind of find the balance point of when you are willing to accept that downtime or when you need

to step out and do kind of a surgical strike.

I do encourage you though not to try and over complicate the psychology AMOLED to include every sort

of use case with court case better off to just leave site dot yml for the kind of heavy lifting of the

configuration tasks.

And if you need something that's really a time sensitive or for us or we can do our operation go ahead

and create a one off playbook that will that's going to take care of that for you for that particular

use case.

Before we wrap up our discussion on the roles I want to introduce you to the ansible galaxy and discuss

external rules.

So here I have an animal galaxy Web site pulled up its Galaxy ansible dotcom galaxy is an online depository

for roles that other people can contribute and you can download and use.

It's very similar to the puppet for the Shepp supermarket.

If you're familiar with those ecosystems if you have a common application that you want to implement

there's a good chance that somebody else has already provided an implementation on the Galaxy that you

can go down and just download and install locally into your Ole's directory and use as is with a little

bit of customisation there structure just like the roles you've been you've been creating.

We've been working with that's just made by somebody else and offer it up online.

So here you can see here how you can use the animal Galaxie command to install the role.

There's also the ability to provide a list of roles inside of her requirements file and having a galaxy

to install multiple at the same time.

We're not going install any rerolls here in this course but you can check the docs here on how to do

that if you're interested in trying it out.

I do want to show you some of the rules here to give you a sense of what that looks like.

Since we've been working with nginx I'll search for an engine next role and and pick one here that we

can find.

So here's one that's got a seems to have a high score.

So there are advantages and disadvantages for using external rolls especially off the galaxy.

So I want to give you some indication of things to look for if you choose to explore this as an option

for getting up to speed really quickly.

So one is How long has the has the world been around and does it have a high rating.


couple of months.

That doesn't necessarily say that it's good quality or low quality but if or if the role has been around

for a long time and a lot of people have used it and raided it there's a good chance that it's stable

kept up to date and you can kind of join that bandwagon and use it and kind of trust that it's going

to take care of your needs because you're not the only one Usie and in testing and providing feedback

to the author.

A second thing to look for is how much coverage does it have of the underlying application.

And what I mean there is if you think about nginx and all the different configuration items you can

change all of the different complexities and that in DOS config files the roll is going to wrap all

of that and present just the variables to you to configure.

So if there is a configuration item you need the author has to plan for that by putting it in the template

and giving you a way to configure that.

So if it doesn't have full coverage of the underlying app you may not be able to configure a certain

role a certain part of that nginx role without going in and monkeying with the role yourself.

That's really not ideal.

I mean the whole purpose of using a roll from the galaxy is that you don't have to mess with the internal

details of how that rolls configured.

You can just use it as a black box.

So it's really a question of how much customization do you need.

How much are you leveraging all that flexibility and the parameters inside of that particular application

and whether or not this role that you find gives you the ability to leverage those features.

And one last thing to consider.

How actively is it maintained against the underlying app so you want to see regular updates as nginx

versions come out and are in are available for great new configuration items will be revealed.

Old ones will be deprecated and the role author needs to make needs to update the role itself so that

you get access to those new config items and the old deprecated ones are marked as such are provided

with backwards compatibility and all of that's really up to the author but it's up to us the consumer

to make sure that that's happening and you can trust that if something new is coming out you get access

to that and it's available to you and you don't have to go in again and modify the roll yourself to

suit your own needs.

So in the end even if you don't choose to use public rolls off the galaxy you can maintain your own

roles within your organization have a sort of private repository of roles and use a lot of the same

tooling to get access to that.

You know in the end we've talked about the main advantage of roles is to minimise the duplication and

maximize reuse if you some encapsulation.

So it's up to you to kind of find the balance point within your Horrigan of getting the speed and getting

up to up to speed really quickly if you use something that's off the shelf so you don't have to build

it all from scratch.

But at the same time getting the flexibility and the stability that you would want to support your application

.

So that balance point is going to be different for every person everywhere.

I'd encourage you to take a look at the Galaxy see what roles are out there and maybe give one or two

or try to get a sense of how it might look for you.
