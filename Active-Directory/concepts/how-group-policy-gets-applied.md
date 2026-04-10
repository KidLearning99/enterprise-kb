# How Group Policy Gets Applied

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: How Group Policy Gets Applied |
| Type | Automated Transcript Synthesis |

## The Group Policy Processing Cycle

In this module, we're going to look at how Group Policy gets applied from the client's perspective.

In module three, we looked at Group Policy Processing from the perspective of you as the administrator defining, linking and filtering GPOs in order to get the right settings to the right computers and users.

But on this module, what I want to do is kind of drill into what happens at the client when Group Policy Processing is happening.

So, let's look at the Group Policy Processing Cycle.

Computer or user objects will start GP Processing and just keep in mind that depending on how it's happening and when it's happening, computer GP Processing might be happening separate from user GP Processing and I'll talk about that in a little bit.

But let's just assume that both are running at the same time in this cycle.

The first thing that happens is what's called the Core or Group Policy Infrastructure Phase.

And, in that phase, the client via the server workstation will query AD just like any client in an Active Directory domain will do to find its closest DC.

Typically, that is done by locating a DC in the same site as the client machine.

And it does that to determine, based on where the computer or user resides in the AD hierarchy, which GPOs apply, which GPOs are linked to this particular computer or user object.

So next, what happen is the client also part of the Core or Group Policy Infrastructure Phase, the client builds its list of GPOs that apply to it.

And this takes into account things, like filtering and takes into account GPOs that have changed or have not changed.

Keep in mind that as I mentioned in a previous module, Group Policy Processing only does work at the client if GPOs have changed.

And what that comes down to is when the client builds its list of GPOs to process, if that list has changed between time one and time two, then at time two, processing has to occur.

So, if nothing has changed in your Group Policy Infrastructure between, you know, one processing cycle and the next, then Group Policy engine will wake up, see that nothing has changed.

And basically, do no work.

But during this phase, what happens is I get that list built of GPOs that apply to me and then I also get the list of client side extensions that need to do work in this phase.

And then we move into the CSE Phase, which is essentially that each client side extension runs, wakes up and does its work for the ones that are called or the ones that required for the previous section or in the previous part of the cycle.

And it processes the GPOs that implement those policy areas of those CSEs in that local site domain OU or LSDOU order that I talked about previously and then the cycle starts all over again.

So that's basically how Group Policy Processing occurs at the client.

---

## Background and Foreground Processing

So, let's move on and talk about the different types of GP Processing.

There really are two different ways that Group Policy is processed at the client.

The first way is called foreground processing.

What this is the GP Processing that occurs at computer startup for computer objects, so when the computer first boots up, it connects to the network.

It connects to Active Directory and it performs Group Policy Processing for the computer and then at user logon for user objects in AD.

There's a foreground GP Processing Cycle that occurs.

And I'll talk about why the Group Policy engine distinguishes between these kinds of processing cycles in a bit.

It definitely matters in terms of which client side extensions or policies areas process when.

But let's just suffice it to say that foreground processing happens for computers at startup and users at logon.

And then there's a background processing cycle that occurs every 90 minutes plus a 30 minute random offset.

So essentially, every, up to a maximum of every 120 minutes.

On work stations, GP Processing, the engine will wake up and perform some processing work and go back to sleep for another 90 plus 30 offset minutes.

And that's designed to process any changes that might have occurred in your policy while the computer has been started and the user has been logged on.

On servers, that actually happens every five minutes.

So you get a lot more frequent checking of Group Policy changes and I think the idea there is that servers tend to be more critical.

And if you make changes like security changes to servers, you want them to get those pretty quick.

Okay.

So we've got these two types of processing, foreground and background.

So let's look at foreground and processing and background processing in a little bit more detail.

So foreground processing, why is it important? Well, certain policy areas can only run in the foreground.

Policy areas like folder direction, softer installation and before Windows 8.1, Group Policy Preferences drive mappings also would only run in the foreground.

In other words, these three areas would only run at computer startup or user logon.

And of course, during foreground processing, just like during background processing, the GP engine doesn't actually do any work unless that list of GPOs that apply to the computer or the user has changed for whatever reason.

GPO has been deleted or unlinked, a security group filter has changed, the user or computer security group membership has changed.

And therefore, the list has changed.

If none of that happens, if the list is the same from one foreground processing cycle to the next, then the GP engine wakes up, runs through core processing and then does no work.

Now similarly with background processing, it's really as the name implies happening in the background.

The user doesn't know it's happening, it's just running, picking up every 90 plus 30 minutes.

And basically, that same logic applies about whether things have changed or not changed and then it goes back to sleep.

So these are kind of the ways that foreground and background processing happen.

The next thing we'll do is look at the different modes for GP Processing.

---

## Synchronous and Asynchronous Processing

Okay.

Let's switch gears and talk about the different modes of Group Policy Processing.

So we've talked about foreground and background.

Now, each processing cycle, whether it be foreground or background can run in one of two different modes.

And those modes are either synchronous, which is essentially meaning that Group Policy Processing for the computer finishes completely before the user can log in.

And then Group Policy Processing for the user finishes completely before they can use their desktop, so that's synchronous processing.

The other kind of processing is asynchronous.

And that means that, essentially Group Policy Processing is running, while the user is logging in for the computer and while they're working on their desktop for the user.

So Group Policy Processing is happening in the background, while the user is getting productive.

And this is a, usually a.

A much better place to be from a performance and desktop experience perspective, but it's definitely different from a Group Policy Processing perspective.

And I will point out that asynchronous processing has been the default on windows work station SKUs like Windows XP, Windows 7, since Windows XP.

And in fact, you have to specifically override it if you want some other kind of behavior.

So let's dig into this a little bit.

So what does asynchronous GP processing really look like? Well, the computer starts up, and this is you know, the cycle from a computer that's off and a user that's obviously not logged on, to a computer that's on and the user's logged on.

So, the computer starts up.

Computer GP processing starts at some point during that boot cycle when the computer gets network access and starts to resolve to AD.

The user's logon dialog is presented.

So the Ctrl+Alt+Delete logon dialog is presented sometime while Group Policy computer processing is continuing on its path.

Now the user can log on at that point, and when the user logs on, then user-based GP processing happens some time in that.

In that user logon cycle.

And the user gets their desktop presented to them and usable while GP Processing may still be going on.

So essentially, user GP Processing is happening in the background and the user is functional and able to work on their desktop.

Now let's contrast that with synchronous GP processing.

So, in synchronous processing, again, the computer starts up, computer GP processing starts up, but the user doesn't get their logon dialog until computer GP processing has completed.

So, once the user gets their log on dialog and they enter in their credentials, they log on.

And now user GP processing starts up at some point in the user logon cycle.

But the user doesn't get a usable desktop presented to them until after the user GP processing has finished.

So as you can see this sort of elongates the whole group policy processing cycle.

So let's look at some of the facts around sync versus async processing.

So, synchronous processing is obviously slower than asynchronous.

I think this is probably obvious but, for background GP processing remember foreground and background, background processing always runs asynchronously by definition, in other words there is always something else going on while background processing is running.

Windows Server SKUs like Windows Server 2008 R2 or 2012, they always run synchronously.

So that's just the default and you can't really change it, it's always synchronous.

On work stations, you can actually force always on synchronous processing by enabling this computer's specific policies.

So it would have to apply to computer objects in AD but you can absolutely get force synchronous processing if you decided you wanted to do that.

And why would you do that? Why would you force it all the time? Well, there are several different policy areas, and I'll talk about this in a little bit, that require a foreground synchronous processing cycle in order to complete.

These include Folder Redirections, Software Installation, and then GP Preferences Drive Mappings before Windows 8.1.

In the absence of running synchronously, let's say you log on and you've got some of these policy areas implemented and you've got asynchronous processing set.

What'll happen is the first time you log on, the GP engine will notice that these policy ares that have been set, and it won't be able to process them because it requires that synchronous foreground cycle.

But it will set a flag that says, the next time the foreground cycle runs, we need to run it synchronously.

So, essentially, you get the situation where you might require two separate logons for the user to receive their Folder Redirection, Software Installation, or GP Preferences Drive Mappings.

Of course, if you're setting the flag to run always synchronous then you're sort of always making the user wait at computer start up and user log on, which is not necessarily a good thing for the user experience.

So let's dig into this a little bit more on one of our test systems.

---

## Synchronous and Asynchronous Processing

So now what I want to do is kind of illustrate what this foreground background sync, async process is all about.

What you see here is, I'm logging into my workstation, and it's set in asynchronous mode, and so essentially I'm processing group policy in the background as I'm logging in, and everything is hunky dory.

Now what I want to do is fire up the GP editor on the local GPO on this machine.

And I'm going to go ahead and set that policy that enables the always synchronous policy processing cycle.

And I want you to notice the difference in essentially how long it takes for me to log in.

So, I'm going to go ahead and enable this policy at always wait for the network, get computer startup and user logon.

There we go.

And now that's enabled.

Now what I'm going to do is, I'm going to go ahead and log off of this workstation.

And I'm going to log back on to it.

Let's go ahead and get back in.

And I want you to notice the difference in the logon time as I go ahead and log back on.

And you see here, now that you saw it was applying registry policy, you saw that message showing up, and essentially what it was doing is, processing policy before it let me see the desktop.

It was a subtle and not very obvious difference.

But essentially, what was happening there is policy processing for the user, my user account, the administrator account, was completing before it let me see the desktop.

And it was a slightly longer log on time.

And if you had dug into the logs on group policy processing, which I'm going to talk about in a future module, you would have seen that the same group policies were applying.

But they were applying at different times.

One was applying while I was getting the desktop presented to me and the other was applying before I got the desktop applied to me.

So, this is kind of a fundamental difference in synchronous versus asynchronous, and I typically recommend to folks, not to turn on the always on synchronous processing, because it can slow up your desktop experience.

You only saw one GPO applying to my user account or my computer account, but in larger environments where you might have five, ten, 15, 20 GPOs applying to a particular user or computer, then this delay in getting to your logon screen or getting to your desktop can be significant.

---

## Group Policy Processing Mechanics

Now I'm going to talk about some Group Policy Processing Mechanics.

So essentially, let's just dig in here.

I want to kind of lay out some rules around how group policy processing actually works, because I've seen a lot of misinformation about this over the years.

And I think it's important to understand what you can and can't do with group policy.

In the presence or absence of things like the network, or access to domain controllers.

So, the first note I will make here is that Group Policy Processing only, only, only runs in the presence of a domain controller.

In other words, if the client cannot find a domain controller in the domain that it belongs to, it will simply not process policy.

So if no DC is found, the GP engine just stops, throws up its hands and say, I'm sorry I can't do any work.

And no changes are made on the target system.

And I think this is kind of an important point because I've seen some people suggest that A way of overriding policy that's being delivered from your domain administrator.

Is to simply disconnect the client from the domain or from the network, make a change to the local GPO and then that change will take effect on the system.

And the reality is, that's simply not true, that even if you make a change to the local GPO, the local GPO is part of the normal Group Policy processing cycle.

And it will simply just be ignored, any change that you make will be ignored, by the client until the main controller gets into becomes available.

And then of course if you try to make a change to the local GPO that's circumventing a domain based GPO.

Because of the LSD OU processing order, your local GPO change will simply get overwritten.

So it's kind of, a key point to remember that, that no Group Policy Processing at all, local or domain-based will happen if there's no domain controller available.

Now, in terms of finding a DC Group policy, the Group Policy engine and Group Policy Processing goes through the normal AD DC locator process to find a DC in the same site.

So it will always try to find a DC close to the client.

And this is a good thing, because as it's downloading policy information from AD and SYSVOL, you want that to be as close to the client as possible to have quick processing times.

And then, contrary to some other popular beliefs that I've seen, Group Policy is not really cached at all for offline processing.

In other words, there's no such thing as Group Policy running while a DC is not present.

And there is actually, in Windows 8 and above, a Group Policy cache that was implemented.

But it is not this, it is not for offline processing.

It really is to simply speed up Group Policy Processing in the case of a synchronous foreground processing cycle.

So essentially, without getting into too many gory details, the way that works is on a Windows 8 or 8.1 machine.

If the client detects that it's doing a synchronous foreground processing cycle, for example you have folder redirection or software installation policy that applies to the computer or the user.

Then Group Policy will have previously cached the GPOs that applied to it and it will use those Policy files out of the cache to process policy to save time.

To sort of make up for the fact that it's a synchronous processing cycle.

But again, the cache is only called when synchronous processing is detected.

So it's not quite the same as like a real cache.

Another kind of behavioral change that Group Policy will go through, is if a slow link has been detected between the client and the DC.

Now, by default, that slow link is defined as less than 500 kilobits per second, which is pretty darn slow in this day and age.

But if that kind of a link is detected and the test is that when the client is doing its DC discovery, it'll do a series of LDAP requests at the domain controller that it finds.

And if those get calculated related out to a response time that equates to less than 500 kilobits per second.

Then it will set a flag in the GP engine that says hey, this is a slow link.

If it's a slow link, than some policy areas will not run.

So admin templates runs over a slow link, and most of the security policy runs over a slow link.

But there are definitely areas like software installation, folder redirection and scripts that do not run over a slow link by default.

Now, you can modify those behaviors.

And also, the slow link threshold itself, that 500 kilobits per second value within this policy area under Computer Configuration Policies admin template system Group Policy.

But by default, those policy areas will not run over a slow link.

And keep in mind that this goes back to the days where 500 kilobits per second was actually pretty darn fast.

And in the case of something like software installation, did you really want Microsoft Office installing over a very slow link? Probably not.

So I don't see these getting hit in this day and age very often, but They are available, and it is good to know that in the case of a slow link, for whatever reason.

You know, you've got a slow link between your client and your DC, you may not see these policy areas getting processed successfully.

Now, Group Policy over VPN is another, I won't call it a fringe case because of a lot of people use VPNs, but it's problematic to some degree for Group Policy.

So if you think about foreground computer processing, which is at machine startup, that's not going to work unless a DC is available.

As I said, and a DCs not going to be available unless that VPN client is pinned up at the time that the machine is booting.

Which is not usually the case, unless you've got an external VPN connection between wherever that client is and your DC.

So that's one issue.

The other issue is that on foreground, user processing, which is at user logon.

This is not going to work either if the VPN is not already pinned up because the DC again has to be available for user processing to complete.

So if you have that option, you've defined a VPN connection in your Windows system.

You have that option, that tick box at the log on screen that says, you know, connect to my VPN at log on.

Then you will get Group Policy Processing for the user, foreground processing, for the user happening.

And again, Group Policy Processing in the background has no issues if the VPN is connected.

So you know, you're booted up, you're logged in, your VPN is connected, and that 90 minute plus 30 minute random offset happens, and background processing kicks off.

Well, that's going to run just fine if the VPN's installed.

But again, there are some policy areas like software installation and folder redirection that simply aren't going to run in the background.

And then finally, some policy area processing differences.

Again, I mentioned some policy areas won't run over slow links, like software installation folder redirection scripts.

Some policy areas don't run in the background, folder redirection, software installation, and prior to Windows 8.1, GP preferences, drive mappings.

And all policy areas won't process unless something has changed in that GPO list.

Then again, you can over ride this on a per GPO basis using that policy area that I mentioned in the previous slide.

So, just some oddities about group policy processing behavior that are important to note to ensure that policies getting delivered the way you expect.

---

## Group Policy Processing Mechanics

So in this demo, I'm going to do a couple things to kind of illustrate what I just talked about.

The first thing I'm going to do is, I'm on my client system, and I'm going to do.

It's called a GP update command, which I'm about to talk about in detail, but it's suffice to say that this is a way of manually forcing a Group Policy update.

And what's going on here is that my Domain Controller is not available so I'm going to be trying to update Group Policy while the domain controller is basically offline.

And we can see what happens when the client tries to run the Group Policy Processing cycle.

So what you see is, it's trying to run user policy updating.

And it's giving this message that processing failed, Windows cannot obtain the name of the domain controller.

So obviously, a lack of a domain controller prevents Group Policy Processing from succeeding completely.

Now the other piece of the puzzle that I wanted to sort of demo is how you can change the behavior of Group Policy Processing.

And so what I'm going to do is go ahead and bring back my domain controller and bring up Group Policy Management Console and I'm just going to go ahead and edit a Group Policy.

And open up under Computer Configuration > Administrative Templates > System > Group Policy.

And you see a lot of the options that I've mentioned in here.

So, I can configure behavior for various Group Policy client side extensions.

For example, if I wanted the script CSC to always process, even over a slow link.

I could click this option here and apply it and any computers that process this policy will always run scripts, even if a slow link is detected.

I could also for security policy, for example, I could go ahead and say, process this even if Group Policy objects have not changed.

And what this effectively does is, for the security client side extension only.

It will run during both foreground and background refresh cycles.

It will run and reprocess all the settings.

So instead of basically doing no work, because nothing has changed.

It will say, it will sort of assume that something's changed and reprocess all of those policies.

And I've seen organizations use that feature, especially for security policy to ensure that if for some reason, somebody had gone in and managed to unset a security setting, a Group Policy Processing would come along, you know, at 90 minute intervals or 5 minutes on server and reapply those security settings.

So these are the kinds of handy features.

You can use some of these configuration settings for, to change the default behavior of Group Policy under different circumstances.

---

## Manual GP Processing

So I want to round out this chapter by talking about a utility or capability that I showed briefly in the previous demo and that's the ability to trigger manual GP Processing.

So outside of our normal foreground and background processing, you can actually force a manual GP Processing cycle to occur.

Now remember, way back in the first module, I talked about the fact that Group Policy is always pulled is always pulled by the client.

And this manual processing doesn't really change that equation.

It's still a pull by the client, but essentially what you're doing with this manual triggering of GP Processing is you're telling the client to manually pull at that point.

And there's really three ways to do this.

There's the Gpupdate.exe command line utility that ships with pretty much all versions of Windows.

There's the invoke GPUpdate PowerShell cmdlet.

That was delivered in Windows 8 as part of the Group Policy PowerShell module.

I'm not really going to go in depth on that in this module, because I have a whole module later on dedicated to Group Policy and PowerShell.

But for right now, suffice to say that that invoke GPUpdate cmdlte can basically do the same thing as the GPUpdate command line utility.

And then finally ,added in Server 2012 and 2012 R2 and Windows 8x is the ability to do a kind of GUI-based GP Update from GPMC.

So this is the kind of a screenshot of the GP Update command and you'll see there's a number of command line switches you can use.

You can either target specifically computer or user policy updating, so you don't have to do both.

The default is, if I just type GP Update it will do a normal GP Update manual GP Processing of both computer and users side policy.

So there's a few other options here, I'm not going to go into all of them.

You can read the descriptions, but the one that's probably most interesting and most often used is gpupdate/force.

And gpupdate/force is job in life is to essentially ignore the rule that says, don't do any GP Processing if nothing has changed.

And it goes ahead and acts as if everything has changed and forces all of the client side extensions to reprocess all of the policies that apply to the computer of the user at that time.

So, it's a good sort of catch all command to issue if you're not getting the policy that you expect to see and you think it might be related to the fact that the computer or the user hasn't yet received that policy.

So a little bit more about the switches and some of the common switches.

So again, gpupdate, no command line switches performs a normal background refresh of GP Processing for computer and user.

Gpupdate/force performs a full GP update, ignoring whether GPOs have changed.

And as an example, if you wanted to just target, for example, all the user side of policy on user configuration of policy for the currently logged-on user, you could just perform a normal background refresh using the /target:user.

Or if you wanted to target the computer, you could just say, /target:computer.

Again, this is done at the client that you want to refresh policy on.

It can't be run, cannot be run remotely.

I'll talk about how you can do that with the GPMC.

But essentially, this is a local command only.

Now, I did talk about GPMC having this new feature in Windows 8x and Server 2012.

And that's the ability to do a GUI-based gpupdate/force on a OU, so you can actually target all the computers in an OU to do Group Policy refreshes on them.

And this screenshot kind of shows you what that looks like.

I've chosen the marketing OU in this case and it's telling me that it's going to go out and detect all of the computers in the marketing OU and any sub-OUs underneath it.

And essentially, do a gpupdate/force on those computers remotely.

So this does require some access to the remote computers, administrative access.

It requires the ability to access the remote computer via WMI, because that's the underlying protocol that it's using to do this.

So there is some firewall holes you may need to poke into your client machine that your trying to update to get this to work.

But essentially, it's giving you that ability to kind of remotely say, go get all of the policy that you need to get.

---

## Manual GP Processing

Okay.

So let's do a quick demo of how these various Group Policy updating tools work.

The first thing I'm going to do is use GP Update and I can get the syntax by using a slash question mark.

And what you see here are a set of options that I talked about earlier.

Everything from targeting, either the computer or the user to forcing group policy processing.

To telling it to wait a certain, essentially offset before it actually issues the Group Policy Processing to.

Log off, or boot.

These are options which essentially serve those synchronous client side extensions I mentioned, like software installation and folder redirection.

Because those policy processing cycles don't run unless a synchronous foreground cycle occurs, which is on a computer, it's a reboot, and on a user, it's a logoff, log back on.

What logoff and boot do, respectively, is set that synchronous flag and force the user to log off.

Or for the computer, the Boot option sets that synchronous flag and forces the computer to restart.

So you have to be sort of careful when you use those options, for obvious reasons.

The Sync option, actually all it does is set the flag.

So it says, go ahead and set the flag for computer user or both, to be synchronous during the next foreground processing cycle.

So if that flag is set and that machine is rebooted then that computer processing cycle that when it comes back up will be running synchronously.

This is kind of a way of forcing synchronous on the next foreground cycle.

So you know, it's easy enough to use the command.

You can just type gpupdate force and it runs both user and computer policy processing by default.

And it tells you when it's succeeded, or if it fails it will tell you that as well.

So let me shift gears and show you the GPMC way of doing this.

Now again, this is a feature in server 2012, 2012 R2, and Windows 8x.

And what it allows you to do is pick a OU.

You can't do it at the domain level, but you can do it at the OU level.

So it allows me to pick an OU, right-click it and say Group Policy Update.

It goes out and looks at all of the computer objects that are in either that direct OU or any sub-OUs underneath it.

And it, in this case it found one computer.

And what it'll do then is if I say yes here, it will go out and do a GPUpdateForce equivalent to all of the clients that it found.

And you can see here that it found my Win7Client and succeeded to deliver that GP update.

If there were errors, it would show lists of those computers that errored out and the error description in case that was relevant.

And that is really all there is to it.

As long as you have sufficient rights, access over W my and permissions to do a GP update remotely you're good to go.

---

## Summary

So let's wrap up this chapter with a summary of what we went over here.

So the first thing is that the group policy processing cycle is composed of two phases.

It's the core phase that does the job of figuring out what GPOs apply to the computer and user.

And it's the CSE phase that basically fires up each client-side extension in turn and processes those GPOs in order to apply the group policy settings.

Group policy processing can run either in the foreground which is at computer start up or user logon, or in the background which is that every 90 minute plus 30 minute random offset on workstations.

Or every five minutes on servers.

Group policy processing can run asynchronously, which is the default in every version of Windows client since Windows XP.

Or synchronously, which is the default on Windows Server.

And that's also, it can be enabled on the client.

And obviously will slow down group policy processing.

And therefore slow down the logon experience and the start up experience for the user.

But nonetheless, there are reasons to do it because of policy areas that require synchronous foreground processing to run.

GP processing requires a DC to be present.

Without a DC nothing can happen with respect to group policy processing.

It just doesn't work.

Slow links that are detected between the client and the DC can change which CSEs will run.

And in that case that sometimes clients had extensions like software installation or folder redirection, simply will skip that time if a slow link is detected.

And then, finally, GP processing can be manually forced, using either the command-line GP Update utility, a PowerShell Cmdlet group or Group Policy's GUI based GP update feature.

---

