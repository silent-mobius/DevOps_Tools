#playbook intro
---
So far in the course we haven't actually made any changes to our three tier application environment

.

We're going to be setting up.

We have a good foundation we've laid understanding how to write playbooks and execute them.

So now we have the framework we're going to start filling in the content from here on out.

So in the section I'm going to provide you with the tasks that we need to use to configure this this

environment in the real world though even though you might get application requirements from other folks

the problem not going to tell you exactly.

These are the things you need to configure ansible to get this working.

So it's going to be up to you to kind of figure out what is it that you need to do to make it work the

way it's supposed to.

And the answer will more than likely it's going to be a lot of trial and error to get it right and experience

as you use ansible over time.

But there's a few things you can keep in mind and I like to call these sort of the four pillars of Linux

application.

And these are things you can look for.

Once you get these kind of taken care of it's likely that you have a lot of what you need in place.

So these four kind of major aspects are the packages that are that they are from a package management

system or a source repository or something you've built yourself the packages to actually lay down the

bits that you need to run the service.

There is the service handler a lot of times this is installed with the package but the upstart the entity

or system descriptor scripts that you need to make sure that your tracking status of that that service

or maybe you use supervisor or some other process manager.

So the service tracker the over all system configuration that supports that so maybe there are files

you need to create or directories you need to create or users you need to set off or certain permissions

sort of like IP tables or firewall rules so think about what the system needs to be configured to support

that application.

And then finally the config files for that app itself.

So there's you know you've got the bits laid down but you need to personalize or customize that app

in some way for it to do the job you need in this environment.

So what config files do you need to touch and modify if you get it to get it set up.

So there's a really broad but if you don't know where to start it all kind of think about those four

pillars and then make some headway on getting your app worked out.

And we're going to use those four pillars as guiding principles for us in this next section.

So as we fill out this content you're going to see the same process repeat itself over and over.

We're going to pick a model for it to figure out which arguments we need to implement and run it see

if it did what we wanted and then move on.

So I'm going to highlight Connelly's modules and the ones that I think you're going to use most often

.

But it's really up to you to go out and figure out which modules you'll need to configure your own application

and your environments.

Show you kind of what that what that process looks like.

Ok so I got the actable docs here that me put them on the screen so I'm going to go to the module index

and here are the documentation links for all of the modules that you can use inside of things.

So you know when you're searching for what Montral is going to be out you know what model is going to

do the job for me what's out there that I can use you can use the search bar and just search for any

label technology concept and you might get a hit on that in the mochel side.

You can also browse through.

Let's look at let's say the packaging modules and say you want to install something with APT.

And so here on the module page VANSELOW modules are pretty good in the sense that they give you a table

of all the parameters that are supported.

If they're required or not where the defaults are and they kind of what the valid values that you can

pass it and then at the end here there's always a list of examples of a lot of different varieties of

ways that you can use this module to do common tasks.

So the examples here are great and looking at them oftentimes gives you some indication of the way that

you should use it.

So when you go through and you pick a module you can read through these here figure out what's required

and a lot of times you might just start with these examples and say That looks like what I need.

Let me copy and paste that.

But then if it needs to be tweaked some way if you feel like you need to expand the configuration or

use something that's non-standard You can go back here and figure out how what what other parameters

you have available to you.

So you know as we're going through this course I'm going to pick out a module and tell you here's how

to use it here's how to configure it.

Just know I haven't memorized all these are the common ones you get to get to.

You get very familiar with over time but there's really no magic.

I mean you come to the mantel index you get to read the docs look at what parameters are there pick

the ones that make sense.

Code it out give it a try and you can do the exact same things you start to pick out the modules you

need to support your environment.

So with that let's continue on with this section and get started with configuring our cluster.



---
# packages
---
So far we've done a lot of preparation and lay the foundation for our closer to get stood up.

Let's start can trigger our cluster now for each of our applications we're going to tackle the first

pillar of application configuration and that's getting the packages installed.

We're going to create a playbook for each of our roles to do this.

Let's start with the load balancer.

So in my ansible directory I'm going to create a new YML file

name and the ballots are dynamic.

Well talk target load balance has worked for our hosts

and supply tasks for load balancing we're going to use the nginx application and we'll look at a new

module now to handle the package installation because we're in a bind to base environment here.

We're going to use the apt module to install our application so the name of the module is apt and takes

a few parameters.

The first one is the name of the pack who we want to install this case it's engine X..

Second is the state of the package and going to use present.

And I'll talk more about the different options for that in a moment.

The final parameter is update cache set.

That equals Yes.

So let's look at some of this some of the parameters here on this line.

So first apt is the name of the module if you're in a different Linux distribution there are options

for yams Zipzer and other package management tools you can take a look at the docs to see those options

.

Going to happen here in our case and after the module the parameter is a it's a set of key value pairs

separated by space.

So the first parameter is name.

As I mentioned that's the name of the package the same way that you would install it if you're on the

command line using apt get install whatever you would put there.

Here's what we're getting at what are you going to put into ansible.

The second parameter is state and there are a few different options.

The first that I the one that I chose here is state is present and present will basically look to see

if that packet is already installed.

If it's not installed it will simply install it whatever the current latest package version is in the

repository.

Another popular option is latest and latest will install it at the latest the first time through but

every time the playbook is run after that if there's a new version available in the repository it will

upgrade that package to the Leighton's.

So depending on your package management strategy and your organization or how you prefer to manage this

you might choose to set it present and just on initial install the package is installed but no upgrades

take place or you may choose to upgrade it every time there's an upgrade available.

And this really depends on your organizational policy or how critical this package is.

That's up to you.

There's also the option of pinning the package version to a particular version the same way that you

could do that on the command line.

And the details of how to do that are in the docs.

And the third option for state that's most most commonly used is absent which would delete the package

or the package if it is installed.

In our case we're just going to install it and allow the version to stay the same after we print the

first time.

The third parameter is update cache.

Yes and this is equivalent to running apps get updates before doing the installation.

So if you're familiar with Ubuntu and the way that it works you need to update your positon the metadata

to see which packages are available.

Once that's done application installs just fine.

But we do need to add up the caches.

Yes just in case this is a brand new installation we've been too and we haven't run an apt get update

first by setting up a cache.

Yes it makes sure that we can find the package in the upstream repository to have it installed.

Let's do the same thing for our database here.

I'm going to create a new file

called this one database that will target the database hosts

for our database application.

We use my siecle server

and this task is almost exactly the same as our nginx tasks.

The only difference is the name of the application.

So now that we have our playbooks written out we're going to execute them in the next and the next video

---

#packaages 'become'

---
All right we have our playbooks written for our load balancer and database rolls.

Let's go ahead and execute them to apply the changes starting with the load balancer here on the control

server in the ansible directory.

I'm going to run ansible playbook with load balancer dot yml.

And we see that it's failed.

Now depending on your local setup this might have worked for you depending on which user you're using

.

Looking at the output here we see that because the task failed we get the contents of standard error

dumped right to our terminal and it looks like a permission to nightmare.

So we talked before about the ansible user and the super user access that was configured for it.

Even though ansible is a single user has super use of privilege we need to tell ansible to invoke that

privilege when executing this task.

So the way that we do that in the current version of ansible is to add it come true.

This could also be become yes or become true.

Capital letters.

There's a few different truth values and that's what septs.

So by adding become true to this to this play it instructs ansible to use the sudo command when executing

this applet installation.

Become could also be added to the task level but by doing it at the play level every command inside

of this this play is going to get Purtell escalation.

In previous versions of ansible become what's called pseudo which still works but that that label is

being deprecated.


.

So now that we've added become true Let's rerun that.

OK.

The command has completed the time that it took the arrow was was the system doing the update because

of the update caches.

Yes.

And then also the nginx package installation itself on to the host noticed that the status is changed

and we didn't get any standard out because the command was a success.

So it's important to note that as we're going through the playbooks now our focus is really changing

away from ad hoc commands and getting the contents of the commands that were run out on the terminal

.

And we just want to know that the command completed successfully.

So when everything is going well we get very terse very succinct out point and we see that the command

status is changed.

Now if we were to run it again immediately

we would expect a few things to be different this time.

One is that it should complete a lot quicker.

And in this case you see that it did because nginx is already installed.

And the second thing to note is that instead of a yellow changed output we see a green.

OK.

And this simply tells us very quickly no drift occurred.

No change was required for that CEP nginx was already installed.

So we just skipped right along.

I kept on with the play.

In our case that was the last task in the place I would exit it.

Let's let's make the change to our database playbook.

It comes true and it will execute our database package installation as well.

All right.

And that package installation completed successfully.

So that's the load bouncer in the database is both completed.

The web search here is going to require multiple packages to support it.

And the next lecture will look at a shortcut that ansible gives us to install multiple packages in the

same task.

Before I leave you though I want to show you the documentation page for privilege escalation.

I'll put the link in the court and of course notes here you can see the discussion of become pseudo

and all the details around the different options here and how you use stuff to take a look at that.

If you have a non-standard antipathies or set of

---
# Packages _with_
---

So far we've installed the packages for load balancer in our database tiers our web server tier has

multiple packages that we need.

So let's tackle that now using what we already know we could write a playbook to install the packages

with four separate app lines.

Let's take a look at what that would look like create a new playbook.

Call it web service.

Just like our other playbooks will target the appropriate to your member to activate or become and set

our tasks and use Apache to as our front end for our web or web service and using apt.

We would install it just like the last packages we've done.

So this gets our first package installed.

Now we could take a naive approach and simply

copy this four times over and swap out the name of each package in our playbook.

But ansible gives us a nice shortcut helper to reduce our overhead and reduce the repetition and it

takes the form of the loop with items.

So instead of typing out four different apps tasks all use our one task and pass it a list of items

that we want to iterate over.

So our first one is attached to.

We also need live Apache to LA Wisky as our interface to our Python stack and to install our Python

dependencies will need Python tip and Python virtual and barne So these four packages we use on our

web servers.

So we need to be able to inject these four items into this apps line.

So instead of hard coding Apache to here is our package name.

We'll use that Jinja syntax item and this item is a reserved keyword that's used with the with items

loop.

So when this is being processed wherever it sees item as adjenda variable it's going to sum out each

one of these.

So we'll execute this task four times once for each of the items in the list.

So let's save that.

And over here we will run Ansible playbook web server will all that's happening.

Let's let's take a look at this playbook some more and let me explain something about the Jinja syntax

.

So first off I'm going to change the name of this task to be more generic so that it explains that we're

installing all of our web components and not just the Apache application.

So as discussed before.

Ansible uses YML syntax for all of its playbooks.

And this this note here this variable here is not Will it's Jinja.

So Ginia is a templating language library that's used in the Python ecosystem.

So what's happening is whenever this YML playbook is being parsed it's being passed through the Jinja

engine to some outer inter-relate different variables depending on the extra logic that's required.

It gives us the ability to use variables and inject variables like we've seen here.

It also gives us some modification filters that we can use to modify the value of the variable before

it's ultimately passed through to ansible for execution.

We have a lecture in the appendix to dive into Jinja more in-depth that turned out familiar with Jinja

.

For the most part we're only going to use Jinja for the verbal injection inside the playbooks and also

and the ansible templates that will cover in a later lecture.

You don't need to know Jinja in-depth or become attuned to expert.

We'll discuss the required concepts as we go through each of the lectures here.

While covering ansible something else note horror waiting on this to complete his You'll see that both


be obvious from this output but ansible allows you to parallelize commands across a number of hosts

.

So in this case we see that you have two hosts inside of our web server tier and you'll see that the

gamma factor step was executed for both hosts together and then the next task was applied.

And so right now.


two at the same time.

All right so we see that the applications patient has completed for both of our app hosts and unlike

the previous load balancer and database package installations here we see in the output that for each

host there were these four items that were looped over and installed.

So we get to see each of the items in the with items loop in the standard output as ansible completes

---

# Services

---

All right we've got our packages installed.

That means that theoretically we have all the bits that we need to configure our services already on

our virtual machines.

Next step let's look at the services reach application.

Now out of the box the application is going to have some default configuration.

And when we install the apps package we're going to get a service script whether it be System D upstart

or old system to be in at the style script.

Those have already been laid down.

So more than likely the services are there and they're running.

We just need to tell ansible about them so that it can track and restart those services when we need

them to.

So let's dive into that a way.

I'm going to start with the load balancer.

So after our package installation I'm going to add a new task

and we're going to use a new module in this case and appropriately called the Service Module.

So the service module takes a few parameters.

The first is name and we're going to give it the name of the service the same way that you would use

on the command line.

So if you type in status in this case nginx or service nginx status that's the name of the service we

want to use here.

Note that it's not necessarily the same as the name of the package.

Next parameter is the state in our case we want state started.

So there's a few different options for State.

Go through the other options after we finish this configuration line.

And the last is enabled.

Yes.

So looking at that state parameter.

So the most common state options you're going to use our state is started.

And in that case what is going to do is when it hits this task it's going to take a look at that state

and say is the service running.

If not it's going to start that service.

Another option is state is restarted.

And in that case and is going to execute a restart regardless of the state.

So I stopped her started and she was going to do that restart the same way you would type in service

nginx restarting the command line.

Another common option is stopped in which case starting running or not it's going to execute to stop

and get get the services stopped.

And then you also have the reload option in there.

All of these states are in the docks under the Service Module documentation.

This last primary that we added is enabled while state has to do with the running state of the service

whether or not that service is running or not currently enabled is the configuration flag for determining

whether or not it starts on start up.

So status started means I want it running right now.

Unable to.

Yes means whenever this box comes up I want it to start on system start up so you can type and able

to yes are unable to know the ending under situation.

So with both state started and enabled Yes this is going to start to service and any subsequent reload

of this VM it's going to start up on start up as well.

So we've got this saved let's execute our playbook

.

All right.

So we see that we built on the previous state install nginx said OK and X is already installed and ensure

nginx started you'll get us is OK as well.

That means that nginx is already running and no extra stuff needed.

Ok now that we know that nginx is running let's hit that and point and see what it returns for us.

Now we can use the built in W get

to test that point and piping it through lest we get it right here on the command line.

The welcome to nginx page so we know that we're hitting the nginx process and we're just getting her

turn that default welcome page.

So in this case we used to get this would've been a lil bit simpler with a curl utility.

My preference.

So I'd like to install Krol and this control machine.

Now the easiest thing to do would just be to get install Currall and go about my day.

But because we're using this infrastructure concept and I want to know that I can get back to this state

on the control machine at any point.

I'm going to go ahead and spend the time and get a real quick control playbook written out so that I

can get it back to this state with Currall installed at any point in the future.

So let's just take a minute.

Let's create a new file it's going to call it controlled I ambles

.

And just like our other package installation tasks and then install tools here using Capt..

And I'm going to go ahead and scaffold it out so that we can use the with items loop in the future we

can just add new tasks without having to add a new whole new line.

Okay.

And with that I'm going to run our control control in my book.


to nginx page again.

And we've got our control set up so that at any point in future if we lose our control machine for some

or some purpose for some reason we can get back and get crawled installed along with any other dependencies

that we find down the road.

OK so now that we've got control and load balancer taken care of we'll add in our service lines for

our last two roles.

So I'm just going to copy our service task here.

Let's go to the web server.

In this case we don't need

nginx but Apache.

So we'll save that when we get that checked off.

While that's happening I'll copy this and again get on my circle process tracked

and the name of the process is my sequel.

I mentioned before the name of the package we installed is my siecle dash server but our actual process

that we're going to track or script is my sequel.

So with that saved let's run our database playbook as well.

All right.

And that's stuff.

So just to recap.

Take stock of where we are.

So far we've got all the packages installed for all of our services and our tears.

We also have service trackers now.

So even though those services were running we now have something to enforce that at this point in the

playbook.

We want to make sure that they're running we've got the service handlers installed

---

# support playbook

---
All right we're making great progress on our configuration management journey.

We have our packages installed our services running although not yet configured.

Let's take a step back.

Before we dive into the rest of our configuration stack and let's look at some operational support.

So in addition to all the configuration management stuff that we have to do we also have this need of

making sure that our application is running and that we can maintain it as it develops.

So let's take a peek at the service manual that we just learned about and write an operational playbook

that we can use to help help us manage our costs or price what we're going to do is create a stack restart

playbook.

So this is a great tool to have in your tool belt that at any point you're troubleshooting or supporting

the stack if you need to know how to get us back to the initial state that I can trust something screwy

is going on and I don't know what it is.

So it's a broad tool but it comes in handy when you're supporting a stack like this.

So here I'm going to create a file and call it stack restart.

I am just going to paste this and but I'll talk through all the steps here.

So we're going to use a service module that we were used previously in the last lecture.

And in addition to the state started parameter we're going to use some of the other states that are

available.

So if you think about the full stack logically if we want to bring down the stack and back up we don't

want to start at the database side because the application might be serving a request that requires

the database.

So we're going to start at the front end here the user bring down the load balancer and then bring down

the application and then bring down the database.

And that as we're bringing it up will go in the opposite direction.

This is a very rudimentary stack restart logic.

We'll expand this over time to make it more robust.

But for now we'll just use that state service module to execute the stack restart.

So looking at our playbook we if we start with load balancer here and we execute a single task we just

want to stop the engine next process we'll do the same with the web server and when we get to my sequel

we could use stopped again.

But since it's the last stop on our journey before we head back the other way we can use state as restarted

just to kick my cycle process and get it rebooted and then back in the other direction we'll use the

web server started and then the load balancer started.

So notice that we're using the hostname is here with the with the groups the exact same way that we've

been using for configuration management.

So even though this is an operational support playbook we don't need to know the names any of the hosts

.

The inventory is going to provide that information for us.

We're just going to go through tier by tier and bring down all the hoes because it's in parallel.

You'll notice that we are going to take downtime using the simplified logic but again in the future

we're going to make this more robust.

But for now at least we have the confidence of we can get a hard reset on the stack and get it back

up to the full running state.

If anything goes wrong

---

# apache2 modules

---

All right let's continue with configuring our cluster.

We've got our unnecessary packages installed.

Our services started when we've written a quick operational playbook to help us with managing the cluster

.

So let's move into some of the actual applications stack configuration to prepare our cluster for receiving

our Python application down the road.

So taking Apache first you might remember that we installed Apache as well as little Apache to mock

whiskey during our first step of getting those packages installed.

Our Python application is going to use a lot of whiskey to serve the requests.

Apache is already running but we need to make sure that the Apache maade Lodoiska is enabled and to

do that we're going to use a builtin module that ansible writes called Apache to underscore module and

it will provide a link to the docs.

You can see the full parameters of of what it takes in the documentation.

If you're doing this from the command line or you would use usually use the A to unlock tool ansible

overrides this Apache module as sort of a wrapper around that same logic.

So jumping into a web server about YML and it could create a new task in our list.

Sure.

Maag whiskey enabled and we'll use Apache to his core module.

And here we use state as presence and name is Wisky So notice that state is present and absent as opposed

to enabled or disabled.

This state is present as a common pattern Mansome.

In our case state present doesn't necessarily mean that the the module is installed or present on the

system state president will actually enable that module in Apache.

So once you enable a module in Apache we need to restart apache before that new model takes effect.

We could use our service module or command that we've used previously and execute this restart but there

would be a problem and the problem is that every time we hit that step in the playbook if we use state

is restarted it's going to restart even if we don't need to.


But even if it's already enabled and we skip right on we're going to hit that status restarted service

task and it's going to restart anyway.

So we're in a bit of a problem here.

And ansible gives us a solution in the form of service handlers.

So the service handler is a special type of task and I'll show you how they work right now.

So instead of adding our service task under the task section we're going to create a new key here call

it handlers and we're going to create the service task the same way we would otherwise.


state has restarted.

So as far as the syntax goes you'll notice this looks exactly like the same way we would configure a

service task in the task list.

So what's different about the handler when it comes time for execution of the handler by default.

Going through this task list this handler will not fire.

So without in the current current configuration even though it's added to the handlers list we will

never see Apache to restart.

We have to request that that restart actually triggers using some condition.

And the way that we do that is using the keyword notify.

So we're going to go back up to our Lodoiska enabled patch to module task.

We're going to add notifiers and the name of our handler restart apache too.

So now what this is going to do is when it comes to this task to enable mod whiskey if there is a change

then it will trigger this notify and then restart apache too.

So if it skips and there's no changed then we won't restart it actually two if there is a change the

Notify will will trigger the other great thing about notify is that you can trigger multiple notifies

throughout a task list in a play and that notification handler is only going to run once.

So if you've got three or four things that need only to restart apache all of those notifications will

collect up in aggregate and at the end of the play that restart will happen.

You can flush handlers in advance if you need to at a specific time before the end of the play.

And I'll link to the documentation to show you just the details of how you do that.

But it's really simple metal flush handlers.

In our case we're fine to let it wait until the end of the play and do the restart then.

So now we've got this saved let's give it a whirl

.

All right so our play book has run and you notice that man reports.

OK so that means it was already enabled.

And our handler work as design even since it was already enabled.


.

But in our case working as designed because my whiskey is already up and running and activated and Apache

when last check here let's Currall tap on.

So you see that we have the default landing page for Apache actually two responding here which is what

we would expect right.

We don't have any configuration yet for what Apache is responding with maade whiskey is going to have

no impact on that default page so we're still setting up some of the scaffolding we need for a python

app.

But Apache suicidal still responding with some default material for us.

All right so we've got our default configuration responding.

Let's take some time now and actually get our pricing application installed and responding in place

.


---
#file copy
---

Continuing on our journey of configuration management the next step is to get our application moved

over to our application hosts and configured and Apache.

So the application that we're going to use in this course is a simple python flask application.

It's going to be served via apache and my whiskey behind our nginx bouncer.

Important note this is this is just a demo application that we're using.

For the purposes of explaining concepts and animal rights we're not teaching Python or flask or my sequel

.

So although it's a functioning app it's purposefully oversimplified and very rudimentary really just

to highlight the way that we would install something into an environment using handsful.

So I'm providing the contents of the entire application inside of a demo app folder and that will be

in the ticket to the coarseness that you can download if you want to use the sample application.

So all of the supporting files that you need for nginx Apache and the python app itself and tip are

all in there.

You can download them.

So this is all going to happen on the web server and to do this we're going to use a new module in the

files section and this module is the copy module which does as it sounds it copies contents from the

control machine to an end.

Most say here on the web server in our tasks section we're going to add a new task copy demo apps source

and copies and launch will all talk through all of these parameters and the second after I've written

them.

So our copy module takes some parameters.

And again you can see all of these parameters on the model documentation on the ansible doc site.

First parameter is source and that's a source that's relative to our control machine playbook directory

.

So you'll see right here we have demo app that's in the same directory as our web server about YML.

So that's the source locally.

And the actual python files are in demo app here so we're going to copy that to the destination on the

app server hosts and we want to create the directory var Dab-Dab up demo.

So you'll notice that we're going to copy them into that directory.

We don't have to pre-created ansible is going to see that that directory doesn't exist.

It's going to create it for us and it's going to copy the contents over and the other thing to note

here is this app with the slash at the end that's important.

If we didn't have the slashy would copy the contents of the app directory in our case we're actually

going to copy the app folder itself with all of its contents and it's going to be located at far.

Dub dub dub demo.


This is equivalent to running a S.H. maade on the host and setting the permissions on that directory

.

There are other primaries you could add like owner and group and many other ones you can take a look

at the docks to see those but this is all that we're going to use in our case.

And finally we're going to add our notification here as well.

So you're going to see this as a common pattern as we build up the state of the app.

Anything that might change the running state that would require a real trigger of the Apache web server

.

We're going to go out and put in the Notify restart apache.

And as we've seen before if no changes are needed the handler won't fire so there's no downside.

But if we do make a change that updates are app the Notify will trigger and we'll get a refresh automatically

as an spools running through the playbook.

In addition to the actual python app source we need to give the Apache configuration that we're going

to use to configure the Apache site to replace the default site and I provided that here in demo demo

dot Dotcom's right here simple patch configuration file for the virtual host.

So again we're going to use the copy module in this case we're copying the Apache virtual hosts config

source is demo demo or dot com destination is going to go into ETSI.

Actually two sites available and we'll set the mode on this one as well.

And again because we're changing the Apache configuration potentially we're going to restart apache


OK with that let's run our playbook and get our application installed on our Web service.

All right.

So we see that we have our change lines which means we had different contents here that needed to be

copied over and actually was going to keep track of the state of the files locally and on the remote

host.

And if there is no change then it doesn't need to copy anything over it would just see an OK.

In our cases the first time we're copying over those files so every file in our folders copy along with


So this is our handler at work.

We had restarted patches to trigger twice for copy demo app source and copy Apache virtual host config

.

But at the end those just collect it up into a single fire at the end of our play and perpetuator was

restarted.

So now we've got application on the web server hosts.

But if we do a curl app one we see we're still responding with that default Apache to page because we're

still running our attitude default configuration.

Before we flip over to our python app site we need to set up our Pipp environment and get our Python

dependencies in place and ready to serve this application.

So we're going to do that next

---

# application modules

---


All right.

So our application files are in place on the servers.

But because it's a python app we're still missing some dependencies that are flask script is going to

require in order to actually serve the the content back to the user.

Now in our web server YML will create a new task set up Python virtual environment

and our pet module.

There's a few things that Pipp can do for us if you're unfamiliar with Pitt.

It's simply a package management tool for Python.

We can pass it dependencies and it will install out of a central repository for us.

You can specify dependencies in a few different ways.

If you have a single package you can ask Pipp to install that.

Or you can put them into a list of files commonly called requirements stop text which I've done for

you here in the demo app directory.

We're also going to use this ansible Pip module to set up a virtual environment which is simply a isolated

container.

It's a folder that puts all the dependencies into this into this folder.

And when we run our application it will run inside of this isolated container.

So it's not tainted by system dependencies or other Trifon applications running on the same host.

You don't have to know much about virtual environment but we're going to use the model to set it up

.

It's really simple and ansible is doing it for us.

So in the playbook we've got our pip module.

And rather than just give the name of the single requirement we're going to pass the requirements

file now because we copied requirements doc text in in the previous step here.

Copy Dunlap's source.

By the time we get down to this step in the task list we know that this file is going to be there and

will gives us that confidence.

So we're going to point to the local file on the app server host where to find the requirements dot

text.

We're also going to pass the virtual environment path now inside of far of dupped up demo are going

to create that D-NV which is my shortcut for saying let's create a virtual environment inside of this

demo app directory.

So inside of this dot venv folder.

Python is going to set up its own tools and install all of our Python dependencies into this local directory

.

So now that we have that

let's remember to restart apache to case anything changes here and we'll run our playbook to apply those

changes

.

All right so now that's done.

We have our pipeline application along with all of its dependencies.

It installed inside of a virtual environment and it's ready to serve content will continue with actually

activating our python site and Apache so we can serve that instead of our sort of our default config
---

#

---

Now that we have all of our Apache and Python application configuration files in place it's time to

activate our python site and deactivate the default Apache site.

To do that we're going to use the File module instead of the copying module to tinker with the files

that are in the Apache configuration folder.

So if you're unfamiliar with Apache the way that the site configuration works is that there is a directory

called sites available and all of your site to vigorish files within there and the ones that you want

to activate are sim linked into the sites enabled folder.

There's a common pattern that Apache and X both use just to show you what that looks like.

We S-sh over in one of our hosts

inside of the Apache.

We have sites available and you can see there is a default SSL in addition to our demo conf which we've


Currently the default conf is the only one that's active because it's sim linked back to our sites available

default clock.

So our job here is to use danceable File module to remove the send link for the default and to add a


sights command line utility that you would use to do this for you if you're doing it from the terminal

and instead of using that because you've already used the command module I'm going to show you how to

do it with a fire marshal to handle the symlink yourself.

So let's drop back onto our control host and jump into our web server YML.

So here we're going to add two tasks one the activate the default Apache site

and we'll use File.

So whereas copy was was meant to take certain files or directories from the local control machine and

put them onto the remote host file is for modifying file properties directly on the host or the target

.

So in our case we want to touch change the files on the target host inside of the ETSI Apache to directory

.


And this is the path to the file that we want to delete and the way we do that is by setting state is

absent.

So with this step we're going to remove that symlink.

And at this point if we left it this way the patch you would have no site configuration and respond

with nothing.

We get an error.

And again we want to restart apache to any time this changes.

After this we want to immediately activate our own site.

So instead of de-activate we're going to activate the demo Apache site

instead of a task because there's not just a single file path that we need to modify.

We actually need assembly from a source to a destination.

So instead of path we use source and the source here is the Apache two sites available demo dot com

.

So that's the source of the send links and we want to symlink to show up in the destination that see

actually two sites that enabled demo comp and the state we want to be like.

So has a few different options for state that you can check out in the docs state.

Present would just create this file at the destination by setting state link.

We say we want a symlink from the sites available to the sites enable.

And again let's get our notify in here to make sure that Apache restarts now that our site configuration

has changed.

OK with that there are ready to roll and activate our pipeline application

.

OK.

So now that's completed our hard demo site should be active in Apache the default site should be removed

and we restarted Apache So we should be serving here.

I'm going to clear the screen.

A quick test here.

We Currall app one we see the response that I've encoded into our Python script.

Hello from sunny.


We can see who's the person that's doing the responding which which apps or is responding to our request

.

We can do the same Kerl app too.

Hello from sunny app too.

So this this tells us that Apache Python Ladue's be all set up all working so that when we Currall from

our control machine we get that Python response at the at the root of the Web servers.

So one last check here if we Kerl LBO one.

We're still getting the default nginx page right.


of that of that server back end and it's responding with its default configuration.

Up next we're going to fix that and point nginx at our Web site Rebecca's

---

#

---
All right.

So we've got both of our application web servers responding with our application Hello page.

But if we had our nginx load balancer we still see the default page getting returned.

So next step is to configure nginx to actually load balance against those web servers on the backend

to do that.

We're going to use a new file module in this case.

It's going to be template now the template is a little bit different than copy in that instead of copying

a file directly from the control machine onto the host.

The template is going to suck out any variable values that we've supplied first and then render the

final content into the host.

I'm going to create a new file such as Let's create a new folder templates in our ansible root.

I'm going to call it next time.

Just to add a J to ending is for Jinja to.

And that means that this is going to be a ginger two templates that Ansible will render first and then

write out once we create the task for it.

So the contents of this file is going to be the engine X-Com syntax with a few variables added in.

So first off we're going to create an upstream.

This is a construct an nginx that says a list of back ends that we can use for load balancing.

And inside of this we're going to use the gender to syntax to loop through

web servers that we have.

OK.

So inside of this open curly percent we're going to say for server and groups web server and this grps

that web server is the same groups variable that we've been using inside of our playbooks.

Whenever a rendering that template we get access to all those same variables inside the scope.

So this for loop is a ginger for a loop that says let's just loop through each one in that list and

for each of those We're going to render server

server.

So in this case for server is the name of the temp variable we're using inside this loop.

And we're going to create this server line for each of those web servers in the back end.

So that's the upstream set.

We need to also create the server that's listening on the on the front end all set to listen on port



content we're going to pass via proxy to its GDP demo.

And this demo refers to this upstream here.

So this is a very standard engine configuration with the exception of our one Jinja to loop that's going

to say about the real backbends in our inventory into this back configuration.

So now that we have that let's go to our load balancer and actually copy this template and get it rendered

and so we're going to create a new task and call it configure engineers site.

And we're going to use the template logic which so similar to the other file modules we have a very

similar syntax.

We're going to specify the source and the sources relative to our playbook here.


to be at c and Jeannette's sites available IMO.

And I'll set the mode as well.

Now similar to Apache we have.

It's available file that word we're going to activate.

We also need to set up a handler for this so that we can restart nginx with the new configuration

and to do that if that actually working to set up our handlers.

Well

OK.

Just like in the case of Apache there's a default site that nginx has been feared in the sites enabled

.

We want to set up using the file resource removal of that symlink for default and an activation of our

some way for our our site for Jews demo here.

So a very similar pattern to what we've used here.

So this de-activate default Apache activate them a site we're going to use that exact same logic here

on the internet service.

So we set up those tasks here

.

So it's enabled the false data is absent

and we'll set up

our task for

our demo site in

this case state as link again.

Once again will notify restart engines.

Let's go over here to our control machine and we will apply our update for the load balancer.

OK.

And now that that's done if we take a look at our hosts we should see if one is responding to is responding

.


So if we run this a few times because this is a load balancer we see that we're switching between app

one and too.

So now our front end is in good shape.

We're hitting our load balancer getting responses from our web servers.

Next up what's called the database connection.
---
#
---

An addition to our Hello index response that I've added to our Python application there is an additional

endpoint that will just test the db connection from the Python application to our my school host.

And those connection details I put into the demo app will show you that file here at Demo dot whiskey

.

So this is the file that Apache uses to activate and run our pipes on the process.

And I'm setting this environment variables that are like her flask to my girlfriend where the app is

going to read and you can see here I've basically hardcoded a string that that can be used to connect

from from Python to my SQL Server and the string here is Demo user demo password at the DB hostname

and connected to the demo database.

So this has been passed in.

And it's it's what's being used by this script here to connect.

You can see it at the time that the script startup's we created a DB wrapper using I think alchemy.

And then when we hit this DB point we tried to run a symbol basically a simple connection against that

that the host and test the connection.

So if we do this now we would expect this to fail because we haven't provided any details to the actual

my SQL Server about the user or this password this database none of that's created.

So at this point we would expect to fail.

And there's another thing though that that's that's going on.

If you look at the the defaults that my sequel has when you first set it up.

So if we log in to the TV host and we run a net daddy and this is a way to check just what all the ports

were listening on the server.



So that means if we try to connect my sequel from the TBO one host we would expect it to work but it's

not listening on any of the external ports.

And so the connection from the application server to the TV host is going to fail even if we get all

the credit of the credential squared away.

So that's the first thing we need to fix.

If we look at my single configuration file that's at see my Seacole my daughter CNF

we see here at the bottom does buying a dress and it's this buying a dress t inside of this configuration

file that's determining which interfaces my sequel's going to listen on.


know about the file and the template we could copy over in my dot CNF file.

We can hard code that value and copy it over intact using copy we could template this out and splice

and variable into that fine address.

But that means then we're tracking the entire contents of this file locally and maybe we don't want

to do that maybe we just need to edit the single file and every single thing else we want to use as

the default and for accomplishing that ansible gives us a handy module called line in file which allows

us to preserve the file as is on the file system.

Or we can just go in and kind of monkey with one line or several lines to change a particular line to

a specific value.

And since there's only one line that we really care about in this file right now we're going to use

that module.

OK so let me drop out of here.

We're going to go back.

We're back talk control host and do the rest for editing from our Anspach playbook.

All right.

So in our database that YML going to add a task



one port.

So it's effectively going to listen on that eats report.

But this configuration will activate my on all ports for the time the syntax for this module line and

file is the name.

Very similar to the other file modules that we've used before we need to apply a destination which is

the path to the file that we want to put it on the host.

And the other parameter we're going to specify is a regular regular expression.

So this rig DXP line is going to take a regular expression.

And in this case we're going to supply

by an address.

So if you're unfamiliar with regular expressions this means look for a line that begins with BIND dash

address and that caret means start of the line.

So we don't want to find out if it's somewhere commented out towards the end of the string just the

particular line that starts with BIND dress.

And then we're going to pass it the contents of the of the value that we want to replace.

So it's going to look for this line that matches bind a dress at the beginning and then we're going

to pass it the string that we want to sub out that line for.

So I'm going to drop here to a new line and the parameter line will be fine the address is equal to


And once that's done we need to restart my sequel.

And for that we need to set up the handload

.

OK.

So now we've got our line in the file line set up

and we're going to execute it.

Now just a note line and file is great.

If you're just trying to monkey with a particular line uncommented line comment a line something like

that.

If you're not concerned about managing the entire file contents which is the case here for us.

So back on the control host I'm going to run Ansible playbook database dot yml.

So

says a quick fabrication we mean we're on net stat on our TV host again.


to accept connections for the My School port from anybody that tries to reach out to it.

And now if we curl our app and point we see a different air new model named bicycle DB.

So for this we need to get some some bicycle dependencies figured out on our Python host how we're going

to do that up next to it are my secret configuration squared away.

---
#
---

In the last election we saw that we were missing a python dependency for Python to connect my sequel

.

We also have some some configuration to set up on my sequel hosts going to do that now using the sequel

modules that ansible gives us.

So first step we saw before that we were missing in my sequel module for Python.

So let's get that squared away back here on the web server host.

I'm going to add one line to install a nice Python dependency here Python sequel DB and I'm going to

go in and kick that off in the background while I talk some more about my sequel modules.

Now following up the dots here for ansible you can see that in the database section of the module index

ansible has a few modules that are dedicated to my sequel configuration.

So we're going to make use of two of these here.

My sequel scores in my sequel score user clicking on one you can see that we can set up a database with

the name with some other parameters here but there is a note that this requires in my sequel DB The

Python package which is what we just install on the web server.

We also need to install on the database so asked for and simple to use this module to configure.

I thought to configure my sequel locally.

So over here on the database hosts I'm going to add a new task to install the requisite tools.

It's going to be an apt task.

I'm going to go ahead and set up a with items Luke.

So if we need to add more calls later we'll have a list already set up for us

.

OK

.

We're going to use the sequel modules My sequel DB and my school user to set up those values that we

require for the connection.

So the first one will configure the demo database

we use my sequel DD is the module name is the name of the database and state as president makes sure

that it's created.

That's very simple and straightforward.

Second module we are going to create the my sequel user demo

.

First off is the user name.

Now the password for starters and getting this up and running we're going to use a simple password that's

plain text and it's the same password that we saw inside of the demo Wisky file that we were passing

to the iPhone application to get it checked it.

We also need to supply a privilege and that's going to be in this format.

So this is all in my simple format configuration.

The name of the database we want us to give it.

In this case it's just the demo database.

All of the tables and all privileges for that database.

We also need to supply a host.

Otherwise this this user would only be able to connect from local hosts.

In this case the DVR one host and bypassing a percent which is the sequel wildcard we allow it to connect

for any host for now.

That includes our top tier one Napster two hosts.

And finally we need a state as president to tell ansible to make sure that this user is created.

OK with that we've saved that file We're going to run our database playbook.

OK.

So let's test now that that connection is actually working.


curl app to TV.

And just like our index endpoint we can connect from the load balancer.

We see that over multiple requests.


So this is a great milestone.

We've made great progress on our journey to configuring our application.

We now have to end connectivity on two of our endpoints our end points for our app but our work is not

completely finished.

So we've gotten a stack working but we've had to make a lot of assumptions and hard coats and values

here.

There there's a few points that are not completely secure in the next section we're going to refactor

our app so that our ansible setup is more robust more maintainable more secure before we do that.

So we want to look at some playbooks that will help us support and maintain this environment.

We're going to do that next


---
#
---

All right our stack is up and we've been using cURL commands so far to verify the status of our end

points.

This is great since we're in the environment we're configuring it step by step we remember where those

points are and it's easy to hit with skirl So it's really fast.

So fast forward into the future perhaps a new member joins the team we're going back to troubleshoot

this environment.

It's likely that you might forget exactly what to curl and exactly what endpoint should be and which

content.

And so we're going to be kind of our future selves and set up the playbook that we can commit to our

repository so that in the future we can just run static playbook and know it's up or it's not up.

This is also a great aid when we're tackling refactoring steps.

So rather than have to go back and individually Currall lots of endpoints or remember which nooks and

crannies we need to check to make sure that full service is working.

We can have the status playbook and any time we make a change we can then go and run the status playbook

and make sure we haven't adversely impacted our environment at all.

We can refactor with confidence.

So let's create a new playbook going to create a new file and put it inside of the playbooks directory

going to call it stack status.

Now if we think about the types of things we want to check one of the things we did early on was set

up services.

So the first thing kind of a most obvious check we can do is check to make sure the services are actually

running.

So let's start there.

So we'll check all of the services in our stack.

Start with the load balancer

service and Jeannette's status.

So we use the service module before to actually start or stop the services the service module doesn't

have an actual status check.

Right.

And if we used the service module we would actually be affecting this the status of that of that particular

service.

So for instance if instead of using the command module we said service name is nginx status started

.

If it wasn't actually started that module would then try to start to service and that's probably not

what we want for a status playbook.

We want it to be read only soft touch just go through and tell me what's wrong.

Because we want to be able to use this playbook in production in testing at any time and know that this

is not going to adversely impact that environment at all.

No changes.

So for that we're just going to use the command module and a simple service status.

And this is going to return one if the service is not running.

And the answer will pick up on that one that Mexico didn't fail for us.

So we'll get an indication that the service is not running and the playbook will show us.

We don't have to actually touch the state of the service.

So we're going to do that for the load balancer but also for the web server tier and that is tier.

So in this case it's not finished next but Apache that we want to check

here it's not genetics but it's my sequel.

Okay.

So with that let's do a quick run and see what that gives us playbooks sac status.

All right.

So everything came back in this case change but because of that command Malda that we're using doesn't

know exactly what content we should return it's just fine that error code.

So in this case with the current state of the playbook everything looks good.

This is what we want to see.

So in addition to the actual state of the service though we actually want to make sure that we're listening

on the right course right because we could start a service but for some reason it be bound to the wrong

court or not listening on the right for it.

So for that we can add an additional check.

Now there is ansible Montral that we're going to use for this and it's called wait for and wait for

will actually stop the playbook and start attempting to connect and checked to see that that particular

port that you provide is listening.

And once it's listening it will continue.

So in our case we really want to know that it's listening.

We don't expect that we're going to be waiting for long for a long period of time to get to the parameters

so that just a quick check.

But you can also use the wait for in other situations and we'll show you how to do that in a few minutes

.

Going back to our playbook just going to add another quick task here

to verify that anyone else is listening on TV and will use wait for and wait for us going to be watching


comes up in our case we don't want to wait that long.

We don't want to know immediately is it going to.

Is it running or is it not running.

So I'm going to set the time out to a very low value of just one second and I'm just going to copy this

down here and we'll do something similar for Apache and my sequel.

So in this case Patchi is also listening on A-T.


So let's run our playbook again and see we can verify not just that the service is running but that

we're actually listening on the correct port.

All right.

And everything's looking good.

So many times as an ops engineer I've been on an incident bridge or troubleshooting an issue and I've

had to ask the question Are we still up or is there a problem.

What's the current status of the environment and that require special knowledge of where to check and

what to curl.

Having a tool like this although it's room entry now you can start to build on this and this gives you

the ability to answer that question immediately with one execution and the other great thing about it

is because it's in the repo and everybody can see it.

Everybody can use it.

Everybody agrees on that question of whether or not everybody can use the status playbook and agree

on it's up because the playbook says it's up or is not up because there's something failing in this

playbook and everybody can contribute.

So if you find a new court case or something else you want to check in the environment added to the

playbook and everybody gets to benefit from that knowledge of how do we know if this service is up right

now or not before we close the lecture we're going to wait for any one of them.

Can one other place we go back and look at our stack restart playbook.

We introduced a very rudimentary logic of saying let's just bring down a load balancer tear down the

web server tier restart my sequel and then start back up.

Now in the real world when we stop and start different services it might take some time before that

completely drains down or when it's starting up the database cure might take a few seconds.

But in our case ansible was just going to keep running on to the next tier and it's possible it's possible

we might bring up the web servers and start passing traffic before the database connection is ready

so we can use wait for for its intended purpose here is add a few hints for ansible to know let's wait

until this process completes before we move onto the next step.

So I'm just going to add a few lines here in Ark in the case of the load balancer.


And the state by default is started.

So we check to see if that port is actually listening but we have a few different options that you can

see in the docks and for the load bouncer.

I mean to say States is trained and what that does is it checks not only that the service is stopped

but that all active IP connections to that port have completely drained down.

So we stop and wait for all the connections to drain out.

And now we've got basically no new traffic or no traffic hitting our service at all.

And we know that OK we've got an environment that's fully down but we can continue making changes and

restarting services trusting that there's no connection that's going to be abruptly dropped.

So for the web server now that we're completely drained we're just going to check to see that the state

is stopped.

So for the web server at this point we know the connections should all be dropped.



and we want to know that it started and will copy the same logic down here into the web server and the


And because I said that states started is actually the default we can remove this line here this line

here.



on to the next.

So now we've got a more graceful restart playbook with SAP.

We'll take a look at some more additions to our Stex status playbook that we can use to get even more

information out of status or custom

---
#
---
All right.

So we've got our stacks set playbook working for us and it's telling us when our services are running

and if we're listening on the right ports hour that's not going to give us all the information that

we need.

If you remember when we were testing these services to first bringing them up when they first came up

they had default pages and they were listening on the right ports which meant from our stock status

perspective that would have been working.

But what we really want to know is that we're actually connecting and getting that content back from

our end points that we want to verify that we're getting the python script that's running and the database

connections work.

So for that we can use a new module called you arrived in Ansible gives us to give us an end to end

connection test for our application.

We pull up the documentation for you right here and it's a model that helps you interact with Web services

and check the contents of different HDTV requests.

You can see here we've got a new Python dependency list you're all you are all Parks's is going to be

taken care of for us.

We just need to get HTP live to installed on our nodes that are doing the connection checks and you

can see all the parameters here just like any of the other modules.

So I'm going to start off by going to the load balancer and we're going to add a step to install tools

the same pattern we've been using in lots of other playbook steps state as president of the cache.

It's


So I'm going to go ahead and take that off load balancing.

So we need to install on the load balancer because we're going to do a check from the load balancer

to the app servers.

So it's going to be executing on your wife's step from the load balancer hosts.

And we also need it on our control host because we'll be executing a check from the control host to

the load balancer post for that first connection.

And we're going to do connections at each step so that when we do our stack status playbook if there

is a failure we'll have very great information about which is the step that's failing right.

Not just that the end to end.

I can't reach something if that's the case that it's failing.

We could also test individual load balancer to web server connections to make sure we understand exactly

which link is the problem.

So here we're also going to install the python HD to be lit to honor control host.

I'll go ahead and run that to get that installed.

So back to our stack status playbook.

We've got all of our service checks in our port checks here in addition to that.

I'm going to add here at the bottom our hand and checks.

So let's start with our first and and test and for this for and run it from the control host.

So I'm going to call this varify and to and

response

and I'm going to use you are I.

And the format for this you are Argile is we passed the URL that we want to hit.

I'm going to use a with items Luke and I'll I'll explain why in a second.

And we're going to do a return.

Content is yes.


successful HTP response.

We want it to give us back the content so we can check to make sure it's not just the demo page or the

default page.

It's actually our our Python application that's returning the content that we want.

OK.

And I'm using this item's loops so that I can do with items group dot load balancer now in our case

when we have one load balancer.

So this is a bit redundant it's a bit extra but this allows us to then if we inject another look at

this in the future and set up an active active raft of passive sort of pair we can we can loop through

all of our load balancers to make sure each of them are returning the content that we want.

Now with this structure right we're going to send the request returned the content and we said please

return the content.

But we need to set a location to save that content so that we can check it in in the next step.

So the way that Anspach allows you to do this is with a certain module certain key called register.

So what we're going to add is register LB index.

So this basically creates a new variable called Elby underscore index and the return content and output

of this particular task is going to be saved.

And then we can use that saved content later on down the line.

In this particular play.

So there is the register.

Now the next module we're going to use this fail.

So now that we've set up this condition to get the query the information that we want we're going to

check the contents of that and fail under a certain condition.

So Fales the Montral will pass a message that we would see in that failure condition failed to return

content on the index.

And we don't want to fail categorically all the time.

We want to fail when.

So this is another ansible logic piece that gives us the ability to conditionally run this task.

So there's a lot of pieces here but you could think of it as we check the content and the next step

we're going to check the contents of that.

And when this condition is FALSE we're going to then fail.

So when.

Hello from sunny.

Not an item not content

with items Albi index results.

OK.

So the with items loop here is because we had a with items Lukan the previous step.

So for each loop in the first step we're going to get a result and hold that in our Elby index which

is actually an array of results.

So we're going to loop through each of those and for each load balance or we find we're going to check

to see if.

Hello from Sunny is in the content that we returned.

And in this case we're saying when.

Hello from Sunny is not available.

We're going to fail so that that's the two stage checks that we've we've implemented here.

Check the content see if it returns.

Hello from sunny anywhere in that content.

If not for this calendar.

So that's end to end what we've done here with all of this logic.

Now we want to do something very similar for the load balancer to check the Bacchantes.

So in our case

we want to

verify the end to end but verify that back in response ROUSSET load balancer is going to become groups

thought web server instead of L.-B this will be the app index.

And in this case we actually have more information about what the response is when we when we get the

Ludd bounce or we don't know which of the two apps services is going to respond here.

Since we're running it from the load balancer against a specific web server we're looping through them

.

We know that we want to return.

Hello from sunny.

Item.

Item.

So this item item here is the same as the item that we used in the last step.

So hello from sunny.

Item by item is going to check to make sure in the case of app one we want to see Hello from sunny and

one case of to want to see Hello from sunny at to.

Otherwise the logic is the same.

I'm just going to change this map on a smaller index results.

Ok so now we've got to sort of end and checks one for the load balancer which is really testing load

balancer into the web server tier.

I'm sorry the end user into the lower bounds of care which is hitting the web servers and then specifically

for each Gallanter web server parent.

We want to check to make sure that that's life as well.

So let's run our new stack status playbook

.

Okay.

And everything is good.

So we see the output here the end and response.


addition to that we can see that this failed message was skipped.

And that's the way the ansible tells you.

I checked the one condition that you asked me to check and it didn't turn out true.

So I'm just going to skip this step.

So we basically have this fail step that's just sitting in the middle of the road there and we check

the condition.

Everything looked good so we just skipped right around it went on without failing.

So one last thing before we finish off this lecture we're going to do this and we're going to check

not just for the index response to hello we're also going to quickly add some checks for the database

by making a few amendments to our checks here.

And we want to check the DB and we'll just change the endpoint to DB.

We'll call this LBB

and the text that we're looking for is not hello from Sonny But database connected from.

All right.

And we'll do the same down here for our load balancer

database connected from

ABC-TV results

.

Typo here

and here.

OK.

So one last go and this should finish up our stock status playbook.

Now that we've had these checks we're basically checking every single link every single connection that

we would have from an end user request.

So we really should have all the information we need to that if something is failing we should we should

find out from the status playbook exactly where that failure is occurring.

So we've got great visibility with the added benefit we could commit this to our repo.

And I don't have to remember which end points to curl.

I can give this to a new team member or to an ops operations person and just say run this that comes

back green.

Everything looks good.

---
#
---

Get a major milestone in our journey our stack is now up and running.

Congratulations for making it this far.

We've completed the basic configuration for all of our services in all three tiers Plus our control

machine.

We've learned about many of the core ansible modules and features that you're going to use in a day

to day basis and Bozek figuration management style and orchestration style infrastructure is code and

we've built to support playbox one for stack status one for stack restart.

Both of those I'm hoping you'll find to be very invaluable tools when it comes time to maintain and

troubleshoot this environment once it goes into production.

However the work is far from over.

There is a concept in agile software development and testing and development that's often attributed

to Kennebec that quotas make it work make it right make it fast.

So we've done a great job of make it work.

So far that's been our main focus in the last section.

We just use the spel features to go from nothing from scratch to get those services up and running.

However along the way we've made some assumptions we've hardcoded values we've put them into just step

by step playbooks from top to bottom configured basically by role.

Part of fixing that now and making it more robust is to make it right.

And as we look to the future we want to set ourselves up so that we can maintain this this code over

time.

We can also make changes without bringing down the functionality of our service.

If you think back to the beginning we talked about configuration management and orchestration.

Actually one of the first lectures in this course one of the key principles was why are we doing all

this.

What's that what's the purpose of investing all of this in Ansible.

And the point is we want to gain confidence in our infrastructure not just in the running state but

confidence that we can go back and make changes without it bringing down to causing problems.

And so those hard coded values that we put in there.

Those are the things that are going to trip us up in the future as we try to make changes and expand

or restructure.

So before we call it a day we move on.

Our stock is working but we need to make it right.

And that's going to involve us going back to make our code more modular and to get all of those hard

could have values out and to replace them with variable values that we can parameterised can inject

new values and that's going to do a few things for us.

One is if we want to stand up a new environment beside this one we can quickly use that modular code

to ingest inject a few new environments specific values and get us the new environment that we need

.

But we can also trust that things like passwords and other secure variables we can inject safely and

organizationally we can reuse these modules if we need to.

I'm going to stand up an entirely different stack with a different application.

We don't have to start from scratch and do a application specific set up for nginx or for Apache.

We can leverage a lot of this code which gives us great reuse great collaboration and ultimately gets

us to a working stack that we can trust much faster.

So as we continue we're going to keep an eye on maintainability and the performance and the security

of our code as we refactor this setup to give us a more robust infrastructure.

Great work so far.

We'll see in the next section.
