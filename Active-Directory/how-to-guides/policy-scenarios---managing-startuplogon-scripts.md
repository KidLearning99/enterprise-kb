# Policy Scenarios - Managing StartupLogon Scripts

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Managing StartupLogon Scripts |
| Type | Automated Transcript Synthesis |

## Group Policy Scripts Capabilities and Limitations

Okay, so we're going to start off this module by talking about scripts and specifically Startup and Logon, Shut Down and Log Off scripts that are available within Group Policy.

And this is a really interesting module I think because it presents information about a feature within Group Policy that frankly is very much of a legacy feature.

It's for sure the most popular, or one of the most popular areas that I see in terms of Group Policy implementations.

But it's also one of the most problematic.

So what we're going to try to do in this module is dive into some of those challenges and some of the capabilities and sort of guide you through.

The best ways and the best places to use Scripts policy.

So let's look at the capabilities first.

So GPO-based scripts have been a feature of Group Policy since it was introduced and of course there have been.

This concept of Logon scripts as way far back as NT4, NT35 you know, Novell NetWare.

So Logon scripts are not a new concept by any stretch and Group Policy sort of took it to the next level.

By allowing you to have multiple scripts running for a given computer or user, and the scripts could be written in pretty much any language and can do pretty much anything you want.

Now principally scripts in Group Policy are used to perform either startup or Logon tasks.

I don't see as many shut down or Logoff Scripts but for example, with the Logon script by and large Drive mappings or even Printer mappings have been the most common usage for Logon scripts in Group Policy.

For startup scripts lots of you know, per machine, machine specific file copies, registry tweaks, software installations, really common in terms of what I see for startup scripts.

And then, you know, Shutdown and Logoff scripts are kind of in their own category.

I've seen them most often used for doing "cleanup tasks".

Maybe deleting temporary files, or you know, saving off some portion of the user's profile during Logoff, or going those sorts of tasks where you want to sort of clean up after yourself, after the user has logged off or after the machine is in the process of shutting down.

Now Group Policy supports both per-computer Startup & Shutdown Scripts, and per-user Logon and Logoff Scripts.

And again, the scripts can be written in anything, any language that you can think of, you can pretty much run a script in it.

That's everything from batch files to VB script, to .exe files to good old PowerShell.

Now there's, as I've sort of alluded to at the beginning of this module, there's lots of limitations in Logon Scripts and in Startup Scripts, so some of those are just a function of the fact that GP Preferences can do a lot of the tasks that Logon and Startup Scripts have been used for in the past much better.

So they're easier to configure.

They're easier to manage.

They're easier to troubleshoot.

Scripts are kind of a black box.

You have to script, you sort of have to be really smart about how you script Startup and Logon Scripts because if a Startup Script or a Logon Script hangs, the user is sitting there waiting for those scripts to finish before they get access to their system.

And the default hang time on a script in a GPO, is ten minutes.

So you've got the user sitting there for ten minutes wondering what's going on, and that's never a good situation.

And for some of these reason and just the fact that scripts run every time and do the same thing every time in a lot of situations, there are major cause of desktop slowdowns.

Microsoft comes out and does something called the desktop RAP, or Rapid Assessment Program and I think the most common result of that shows that Startup or Logon Scripts end up causing a majority of the slowdowns that you've experienced.

That's certainly what I've seen in a lot of environments.

Now Scripts are good if you need, what I call logic flexibility that scripting can provide that you can't get from GP preferences using for example, item-level targeting or Policies in terms of how they deliver what their functionality is.

So, let's look a little bit about what these Scripts look like.

So, Shutdown and Startup Scripts are found under Computer Configuration Policies > Windows Settings > Scripts.

And this screenshot just shows you the main dialogue that is available when you, for both Startup Scripts and frankly for Logon scripts, or Logoff, you have this dialog that lets you add Scripts and the Scripts can have parameters.

And you can add PowerShell Scripts on a second tab, and I'll show you that in a little bit.

But essentially, you know these two dialogs are really straightforward, really easy to use.

The only tricky part is in where you.

Where you put the Script Files.

You have a couple choices.

I will talk about that for sure but if you see the Show Files button on this, what happens when you press that is it opens up a Explorer Folder within the SYSVOL portion of the GPO, so it's the default location for Scripts, Startup or Shutdown or Logon and Logoff.

Is in the GPO storage, the setting storage that's found in SYSVOL.

So, you know, you can put the files there or you can put them on a separate UNC paths somewhere else on your network; it doesn't really matter.

The point is, that is the default location.

So let's dive in and kind of poke around in the scripts area for a little bit and see what we can find.

---

## Navigating Group Policy Scripts

Okay so let's take a spin around Scripts Policy and see how we can get started with this.

So I'm going to go ahead and Edit a GPO I created called Scripts Policy.

And I'm going to go in under Computer Configuration > Policies > Windows Settings > Scripts.

And then double-click the Startup Scripts area and you'll see that I have a few options here.

If I Show Files, what this does is it actually brings up the default location where you can store scripts and you'll notice that it's in a SYSVOL path.

Remember, the SYSVOL is the folder that's replicated in all domain controllers.

It's where most of the policy areas store their settings for Group Policy.

And you'll see here, this is the GUID of the GPO, and the fact that it's in the machine folder indicates it's a per-computer Script and under Startup.

So I can't actually create or drop a folder in here as I'm referencing it right now because I'm referencing it according to the domain DFS share, and it's going to give me a read only error.

But it's showing me the option I have to see or select, or at least view what scripts are currently available in this folder.

Now when I go in under Add, this is where I add a reference to the Script that I want to run.

And if I go ahead and browse, you'll notice it takes me to that same location again.

And from here, I can actually go right-click Text Document and I can create a Script in here.

I can call it logon.bat and I can actually Edit it.

And I can say, put in a net use z: \\ pointing to r-2test.tld\user\public and go ahead and Save that.

And select it by clicking Open and I've now got the script here that I selected.

So the other option you'll notice here is support for Script Parameters.

If you had a script that took command line parameters, you could provide them in this block here.

And it would get passed to the script as it executed.

It could be an executable that takes command line parameters and the same holds true here.

So if I were to click OK here, then this logon.bat reference is added to this GPO and it will run at the next boot cycle.

So, I'm just going to go ahead and Cancel out of this.

First I want to just show you quickly, the PowerShell section.

This was added, as I mentioned in Windows 7 and Server 2008 R2, and it allows you to have PowerShell scripts, just as you can have regular other kinds of scripts, or executables.

And you can through this dialog, you can actually control whether PowerShell scripts run first in the GPO versus the regular scripts that are from this tab right here.

So you can control that order if you need to.

So that's just kind of a quick introduction to how scripts works, and I'm going to dive in and give you a little bit of a scenario here in a bit, but I wanted to kind of introduce the UI, and sort of how to navigate it.

---

## Deploying StartupShutdown Scripts

So now I want to talk about Startup and Shutdown Scripts in particular.

So let's talk a little bit about the capabilities and the limitations in Startup and Shutdown Scripts.

So the thing to understand about Startup and Shutdown Scripts, they're running per computer, so they're running before the user is logged in.

And that means that they have to run in some kind of security context.

And as it turns out, they run in the LocalSystem context.

Or, essentially, they run as the machine account.

So, this allows you to do pretty much anything you want in a Startup Script, in terms of managing the configuration or making changes to a Windows computer system.

So what that means is you can change registry keys that the user wouldn't normally have permission to.

You can update files that the user wouldn't normally have permissions to.

Any privileged access is available to you within the context of a startup script.

The other thing to recognize is, is if that Startup Script is trying to access resources on the network.

For example, if you're trying to copy a file from a network share to the local system within a Startup Script.

Then, the context in which that access to the network is happening is essentially the machine account.

So, the machine account has to have permissions to the share in order to be able to do that copy, and that's something that's important to keep in mind.

So, again you can, these Startup Scripts and Shutdown Scripts run at boot, or at shutdown.

And in the case of a Startup Script, don't require the user to be logged in.

It's pretty self explanatory, but I just wanted to underscore that.

That, in fact, the Startup Script runs before the user even gets their log on prompt, to log in.

So, what are some of the limitations? So, it does require a reboot or a shutdown-restart to log on or to generate or to execute a startup script.

So, that reboot is kind of the trigger for executing Startup Scripts.

Startup Scripts in and of themselves don't provide much in the way of logging.

If you run a RSoP report, or a result set of policy report and I talked about that in the earlier module, against a system that has received a startup script, you only get the last execute time.

So you only see what actually happened or when that script actually ran last, so at least you can see if it did run.

But the downside is you can't, you don't get any logging of what it did, because it's a script.

It could be doing anything.

And, it will run every time, regardless of whether you need it to.

So, in other words, whatever you're doing in that Startup Script whether it's doing a file copy or doing a registry hack, it's going to run every time that system reboots.

And it's going to redo the file copy and redo the registry hack unless you have coding in your script that tests for the thing you're trying to do to see if it's already been done.

So scripts don't have any intelligence in them other than what you put in that script.

And that's a really important point about all of the script capabilities in Group Policy.

You need to think about what you're doing in your script.

And ideally, you want to add the logging and the testing of whether it needs to run, so that it doesn't do extra work.

And so that if it does have problems, you know what's going on.

So let's look again at the UI and see how this works.

So remember I mentioned that if you hit that Show Files button, that browses to the folder in SYSVOL, which is the default location to store startup or shutdown scripts.

And the Add button will add the reference to the script into the GPO, and the script could be in SYSVOL, in that startup folder, or it could be out on a network share somewhere.

It doesn't really matter.

The advantage of keeping it in the GPO and SYSVOL is that as long as the GPO is available, the script is always available to the machine or the user that's executing it.

And from a permissions perspective, you probably have less challenges around managing access to the script.

With the Up and Down buttons, you can change the execution order of the script.

So if you have multiple Startup Script, which is perfectly legitimate, you can change the order in which they execute based on that Up and Down button.

And then finally, if you hit the PowerShell Scripts tab, you can add PowerShell scripts.

And these are differentiated from regular scripts just by virtue of the fact that they have a PS1 extension and they run in the PowerShell engine.

So they're treated differently from a scripts perspective in Group Policy.

So let's dive in and kind of show you how to deploy a startup script and what it looks like.

---

## Deploying StartupShutdown Scripts

Okay, so now let's deploy a startup script.

So I've got a GPO called Scripts Policy, and I'm going to go ahead and edit it.

And go in under Computer Configuration > Policies > Windows Settings > Scripts, and I'll double-click Startup.

And, if I go Show Files, I've got this logon.bat file that I created in a previous example.

I'm going to go ahead and delete that, since I'm creating a Startup script here.

Actually, I can't delete it from this location, so I'll go ahead and go to the Add button.

I'm going to say Browse, and I can go ahead and delete it from here.

Now, I'm going to create a new script called startup.bat, and I'm just doing this within the Explorer browser because it's easy to do.

I could go ahead and do this from within the Explorer if I wanted to, but the fact that it takes me right to the location in SYSVOL means it's easy for me to do this up here.

Now what I'm going to do is I'm going to paste in a command and all this is doing is it's using the reg.exe command to add a value to the registry under HKEY_LOCAL_MACHINE\Software\TestApp, and the value is called Enabled, and I'm setting it to 1, with a type of REG_DWORD.

And this is a good example because ordinarily a regular user would not be able to make a modification to HKEY_LOCAL_MACHINE.

They're permissioned away from that.

So I'm going to go ahead and save this batch file.

And then, I'm going to select it by clicking Open here.

And you'll see I've selected startup.bat.

I don't have any Script Parameters on this particular script, but I could use that if I needed to.

So, I've got my script, I'm going to go ahead and click Apply.

And now, because this is a Per-Machine Policy, what I'm going to do is I'm going to come up to the Marketing OU which is where my computer account is and I'm going to link that Scripts Policy to the Marketing OU where my computer account is.

Remember it's per computer so I need to do that execution that linking at the OU that contains computer objects.

Okay, so now what I'm going to do is I'm going to go to my Windows 7 client and restart it.

Remember, I need a restart in order for the machine to actually pick up the script.

So I'm going to go ahead and do the Restart.

And we'll see what happens when the machine comes back up.

Now, my machine is coming back up.

So we're going to go ahead and see if we can see the execution of that script.

So it's applying computer settings.

And now you saw it flash by, it applied scripts.

Now I'm logging back on to the machine, and I'm going to go ahead and verify that I received that Scripts Policy, by checking the registry to see if my registry entry made it.

So I'm going to go ahead into the Registry Editor.

And if I come in under HKEY_LOCAL_MACHINE SOFTWARE, there's my TestApp key, set to enabled with the value of enabled is set to one.

So my script was delivered.

Now, remember I mentioned that you can use RSoP to get validation of some of this information.

Let's go back to my server and I'm going to show you a little sneak peak of what we're going to be talking about in a later module around troubleshooting.

I'm going to go into the Group Policy Results wizard.

And I'm going to collect against the Win7 client that I have in my domain that I just delivered a startup script to.

And I'm going to say, don't display user settings, I'm just interested in the computer settings.

And once that report runs.

We'll go into Details, and we'll go down to Settings, Startup, and you'll see the Last Run time is listed here, and so that tells me that the script actually ran, and it was delivered by the Script's Policy.

So I've got the capability of delivering Startup Scripts that can make changes to the system that users can't make.

And I can verify that those are at least being delivered using Result Set of Policy or Group Policy Results.

---

## Deploying LogonLogoff Scripts

Now let's look at Logon/Logoff Scripts.

So very similar to Startup and Shutdown, Logon and Logoff Scripts run in this case, the user's context, because the user has to log on in order for Logon Script to, or a Logoff Script to run.

So it's good for making changes to user settings and data.

So if you need to make a change to HKEY_CURRENT_USER in the registry, or you need to copy a file into the users profile like a shortcut file or something like that, it's the right place to do that kind of thing because it's essentially running as the user.

Now, it does run at logon and logoff, so it obviously requires the user to logon to execute and on the downside, it does require the logon/logoff to run, so you have that trigger point, that, that only trigger point is the Logon/Logoff cycle.

Again, just like startup scripts, it's up to you to provide logging in your Logon Scripts just to find out if they really need to run again, or if they've done something wrong.

You know, one thing that you need to be aware of with Scripts in general, in Group Policy, is that there's a default time-out on the script actually executing and completing of ten minutes.

What that means is that for some reason, you've done something in your script that's causing it to hang, then your user could be sitting there literally for ten minutes waiting for the script to execute before they're able to move on with their desktop.

So that's something to keep in mind.

And really when you're writing your scripts, it's easy to write a script that just does a dry mapping or a registry change.

It's a lot harder to write a script that the test to see if those things need to be done and then logs that they're doing it correctly, and then having exception handling in the case that they can't do it correctly.

And so that's what I always recommend, if folks are going to use scripts, that you use them correctly because the downside of scripts is they can cause really bad and hard to troubleshoot delays for the user.

And again they'll run every time regardless of whether they need to unless you script around that.

So I showed you the UI in Startup and Shutdown Scripts and I showed you the General Scripts tab.

Well Logon/Logoff Scripts have the same identical UI so what I wanted to do in this screen shot is show you the PowerShell Scripts tab.

So if you were going to add a PowerShell reference you would use that Add button and essentially it would browse you into the sysfall portion of the GPO just like it did for the Logon Scripts.

And or I'm sorry Startup Scripts and you'd be able to add a reference to a .ps1 file.

You can also set the order of script processing so you can control whether PowerShell Scripts run first or last within this GPO.

And that's in the case of let's say you have some scripts that depend on other scripts, you might have regular scripts that depend on PowerShell Scripts or vice versa, you can control which scripts are specified to run first within this GPO.

And that's useful if you have that scenario.

I don't know that I would necessarily recommend having multiple types of scripts running in a single GPO unless you're like I mentioned earlier doing a lot of good logging and testing to make sure that those scripts run properly.

But you do have that flexibility within this tab.

So now, let's go and implement a Logon Script, and I'm going to try something different.

I'm going to use a, a PowerShell-Based Logon Script in my next example.

---

## Deploying LogonLogoff Scripts

All right.

So now, I'm going to implement a Logon Script Policy.

So, I've got my GPO created call Logon Script Policy and I'm going to go ahead and edit it.

And this time, I'll go in under User Configuration, Policies, Window Settings, Scripts, Logon, and remember I mentioned I wanted to demo using a PowerShell Policy, or a PowerShell Script as my Logon Script.

So, I'm going to go ahead and click on Add, Browse to the location in sysfall.

And I'm going to go ahead and create a PowerShell Script.

So, I'm going to call it, logon.ps1.

And, Yes, I want to change the extension.

And I'm going to go ahead and right-click and Edit.

And I'm going to go ahead and open the script.

And it opens by default in the PowerShell ISE which is the script editor that Microsoft provides for PowerShell scripts.

I could have associated the .ps1 with Notepad but in this case I'll just go ahead and use this.

And I'm going to go ahead and paste in a command, that, what this command does is it sets in a user environment variable.

The variable is called UserVariable.

And it sets it to the value of Hello.

So, I'm essentially getting this script is going to execute a user variable change for the user when it runs.

So I'm going to go ahead and save it.

Close this out.

So now I have my PowerShell script.

I'm going to say Open to select it into the GPO.

And I could set this order.

Since I don't have any other scripts here, I'm not going to bother.

But I'll go ahead and leave it at Not configured.

Say OK.

And in this case, because this is a per user policy, I need to link it to where I have my user accounts.

In this case, I've got my Good Old Joe Sales in the sales users OU.

So I'm going to go ahead and drag this GPO up to the users OU and link it.

Now let's go to my Windows 7 client.

And remember, I need to log off and log back on for this policy to take effect.

So I'm going to go ahead and do that.

Go ahead and log off.

And now I'm logging back on and we can see now as it's applying user settings, we'll see if we can catch it at run scripts policy.

It went by really quick but it did run.

And now what I'm going to do is go ahead and run the command prompt and type set.

And remember my variable was called UserVariable, and it was set to Hello, and there it is.

I can highlight it here so you can see it.

So User Variable=Hello has been set.

And the script has executed.

So, we've delivered a per-user setting, to this user, via logon script.

And again, both of the things that I've demoed in this module around startup scripts and logon scripts could both have been done using Group Policy preferences.

Registry values can be modified using preferences, or admin templates, for that matter.

Environment variables can be modified using preferences.

But, the point here is that scripts does give you the capability if you need additional logic beyond what Group Policy can do for you.

It does give you the capability to deliver these per-computer and per-user settings or changes.

---

## Script Configuration Options

I want to finish up this module by talking about some options that you have for modifying the execution of Group policy based scripts.

So, there are actually some settings within admin templates that allow you to configure script behavior.

These are both per-computer and per-user, and they're not the same options in both but some of them similar that the user configuration options have fewer settings available.

So under computer configuration, what you're seeing here and on the screen is a set of options to specify different script behavior.

And I'm going to point out just three settings that probably are the most commonly used and the most useful.

Just so you know what they do and how you might be able to use them.

So the first one is specify maximum wait time for Group Policy scripts, and this is a way, remember I mentioned that scripts by default take ten minutes before they actually time out.

So if a script is still running ten minutes after it started, it's eventually it's going to basically go away.

It will be killed by the system.

What this policy lets you do is change that default ten-minute time to something greater or less than ten minutes.

And I've seen this used in a lot of situations to reduce that possible timeout.

So if you do get a script that hangs for some reason and you're not handling it in your script code, you can at the very least, you can see that the script will kill itself in a shorter period of time.

You know, in most cases, if a script is taking even as much as a minute to run, you're probably doing too much in that script because this is obviously impacting the user's ability to get their job done, get the machine available to them.

So, I highly recommend modifying this down to some value that you think is reasonable for your script environment.

The other policy that I think is useful to use is the run logon script synchronously.

This is so let me just describe how scripts run normally.

If you have multiple scripts running in your environment, they will run asynchronously, which means that they're not going to necessarily run one after the other.

They're going to run at the same time, if you have multiple scripts.

They're going to kick off, one after the other, without waiting for the first one to finish.

If for some reason you need scripts to wait for the first one to finish, then you can set this run logon script synchronously, for those logon scripts, and also startup scripts.

There's an option for that below.

You can set those to run synchronously, or, in the case of start up scripts, asynchronously.

So, startup scripts are a little bit different.

Startup scripts run synchronously, by default, and you can set them to run asynchronously.

So a little different behavior, two different policies for controlling them.

So if you want logon scripts to run synchronously, you enable that policy.

If you want startup scripts to run asynchronously, you enable that policy.

And these are just a couple of different options for controlling the order of script processing or controlling the behavior of script processing.

You'll notice there are scripts around PowerShell execution order.

Instead of setting it in the GPO, you can set it at the Group Policy level, and it will essentially set it for all scripts in that order.

And this is just a little bit of control that you have over script execution that you might want to consider using if, for example, in the timeout case here, your timeouts are too long.

So now let's summarize what we learned in this module.

And in the next module, we're going to talk a little bit about a special kind of configuration called loopback processing.

---

## Summary

So, to summarize what we've learned about scripts in Group Policy.

Scripts come in two flavors.

There's the per-computer startup and shutdown scripts and per-user logon and logoff scripts.

And you know as I have mentioned the role of scripts and the things that people have used scripts for in the past have largely been usurped by Group Policy preferences.

But scripts still have a role if you're needing to perform complex logic.

So if you need to do conditional testing, if you need to do things that are just simply not available in Group Policy preferences then scripts still probably have a role.

But again, remember that scripts are only as good as you write them, in terms of logging and logic to ensure that they don't run when they don't need to.

So all of those things sort of mitigate the value of scripts unless you can guarantee that they're doing the right things.

And again, they are a big source of slow down and complexity in an environment.

If you don't have good logging or good script execution testing going on because they're kind of a black box.

You saw how I showed you the Group Policy results data that shows at least, that the script executed which is at least something.

But it doesn't show you at all what happened during script execution.

So you only see the results of it.

And then, as of Windows 7 and 2008 R2, scripts can now include PowerShell, and PowerShell scripts, you know, can have their own tab and can run in addition to regular scripts.

One thing to note about PowerShell scripts is that the execution policy by default, on a windows system out of the box is set to restricted, which means that you have to sign the script in order for it to run.

So if you need a different execution policy, and this is going to affect logon and startup, and shutdown and logoff scripts as well.

You're going to need to set the execution policy before you put your PowerShell scripts into Group Policy.

And then finally there's some, script execution options in admin templates that I showed you, that can control everything from that maximum wait time to whether log on scripts run synchronously or startup scripts run asynchronously.

So you have some options there in terms of controlling script execution.

So as I mentioned, in the next module, we're going to cover kiosks scenarios and so-called loopback processing.

So let's dive into that.

---

