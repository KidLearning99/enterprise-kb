# Introduction to Group Policy Scripting

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Introduction to Group Policy Scripting |
| Type | Automated Transcript Synthesis |

## The Microsoft Group Policy PowerShell Module

In this last module, I'm going to talk about and introduce the concept of Group Policy Scripting.

Now primarily, this is going to be a PowerShell discussion, but I will touch on some capabilities that are still available via VBScript that provide some neat functions and neat capabilities within group policy management.

So, Microsoft, since Windows 7, has provided this PowerShell module for Group Policy as a function of GPMC.

So, when you install GPMC on Windows 7 or as it comes installed by default in Server 2008, Server 2012, and Server 2012 R2, you will get the commandlets, the PowerShell commandlets that come with the group policy module installed by default.

So once GPMC is installed, you're all good to go.

Now, just for information's sake, prior to the Microsoft module coming out, I had built a free group policy, GPMC PowerShell module that was available for Vista and XP and you can still download that.

There are some Commandlets in there that Microsoft doesn't still yet provide in their module.

So, if you go to GPOguy.com, you can get those Cmndlets, and, use them, still on XP and Vista, but also on the newer operating system versions like Windows 7 and Windows 8.

So, included in the Microsoft module are 26 commandlets.

And the command, the module itself is called GroupPolicy, one word.

And it's primarily geared towards the management of GPOs and their links, and their permissions.

And there's a few other bells and whistles in there, including some limited support for reading and writing actual GPO settings.

And I'll talk a little bit more about what that limit is and what you can and can't do with the command lets.

So, this is just a screen shot, if I type get command dash modal space group policy lists out all the commandlets and a couple aliases within the group policy modal on server 2012R2.

And these 28 commandlets, really, 26 plus the 2 aliases in Server 2012 R2 Windows 8.x provide capabilities over a range of GP Management Tasks.

So if you call that PowerShell has this kind of verb noun structure, Get GPO, Backup GPO.

These are all the verbs available under the GPO noun.

Backup, Copy, Get, Import, New, Remove, Rename, and Restore.

In terms of GPO permissions, you can Get and Set permissions and these are permissions on the GPO itself.

You can get or set GP inheritance, and this is really just a matter of getting or setting that flag that you can stick on OUs and domains to block inheritance.

So, you can either get the value of that OU or domain or set the value.

For GPLink, which is the, a link on a site domain or OU, you can create a new link, you can remove a link, and you can set the link.

So you can change the properties on a link using the set verb.

With GPOReport, this is simply a Group Policy settings report in either HTML or XML format.

You can get that, and you can get or create new starter GPOs.

So you can create a new starter GPO using the New GPStarterGPO commandlet.

Invoke GPUpdate.

It was actually added in WIndows 8 and beyond.

And it is basically a remote group policy update or GP update.

So you can literally invoke a remote GP update, just like you can from the GUI in GPMC, you can do it from Power Shell.

The get GP result instead of policy essentially lets you run an RSOP report against a remote system.

So you can get RSOP data from that command one.

And, the final two verb noun combinations are get, remove, and set GPPref registry value and get, remove, set GPRegistryValue, and they respectively let you set or get GP preference registry settings, and admin template settings.

That GP registry value lets you set admin template settings.

So, I'm not going to go into that.

I sort of consider those to be a little bit more advanced commandments.

But those are the limitations right now for being able to modify settings.

So let's kind of look overall at the limitations in the current commandlet set so that you know what you can and cannot do with the Power Shell commandlets.

So today there is really no support outside of the two areas that I just mentioned Admin Templates and GP Preferences Registry if no support for readying or modifying settings in Power Shell from Microsoft.

There's no support for retrieving GPO links.

So, for example, there's no Get-GPLink.

You can set links, you can create new links, but you can't get links.

You can't get information about existing links which is, I think, a big oversight.

And by the way, my GPMC Cmdlet, GPMC module does include a get GP link function.

No support at all for creating, linking, editing, or deleting WMI filter, so you can't really do anything in Power Shell with respect to managing WMI filters.

No support for setting the GPO status.

It can be done, but it's an indirect thing.

So there's no Cmdlet set that says Set GPO status, for example.

No support for managing the delegation other than the actual GPO delegation or the permissions on the GPO.

So, if you remember from my module on GPO delegation, we could set delegation for things like who can link to which OUs.

That's not supported in the command links.

I will say that a lot of the functionality that Microsoft provides in GPMC is available through a COM API that's used indirectly by these Power Shell commandlets and you can use Power Shell to interact with the COM APIs directly to get access to funcitonality that's not available in the Microsoft commandlets.

Now, I'm not going to cover that again because it's a little bit more of an advanced topic.

But there's lots of information out on the Internet about using the calm APIs and GMPC directly from PowerShell.

So let's kind of dig in and look at just sort of take a tour around the Group Policy module and see how it works.

---

## The Microsoft Group Policy PowerShell Module

Okay, so let's go ahead and take a spin around the PowerShell Group Policy Module in Server 2012 R2.

I'm going to go ahead and start PowerShell, but when I do it I'm going to run it as administrator because there are certain operations I'm going to want to perform against GPOs that are going to need my full admin permission.

So, I'm going to go ahead and do that.

Now, in PowerShell V4, Microsoft introduced this concept that's sort of on-demand module loading, where you could type the name of a commandlet, and if the module wasn't already imported, you could, it would automatically import it by default.

But if you're not in that version, you can just type import module, name, group policy, and the module gets imported.

Now, remember, I typed Get Command Module Group Policy.

And it gives me a list of the command lists and aliases that are available in this module, in this group policy module.

And so the simplest one that I'm going to start with is Get GPO.

So Get GPO lets me get a reference to a group policy object.

So, I'm going to, typically, I'm going to use the command lit.

Tab completion to show you some of the parameters that are available here.

So I can provide a name, I can provide a GUID, I can provide a domain if I want to get access to GPOs in different domains other than the one I'm currently in.

And I can tell it which server or DC I want to connect to, to get this information, so I can specify a DC.

I can also, with Get-GPO, use the -All parameter.

And what this is going to do, if I hit Enter, is, it returns a list of all the GPOs in my domain.

And you'll see that it's got, for a given GPO, the display name, the domain name.

The owner of the GPO.

The GUID or ID of the GPO.

The status of the GPO.

Any description which is the comment field, create and modified times, and then the user and computer versions and any WMI filters that might be linked to this GPO.

Now if I wanted to just get information about one GPO, I could just type -Name, and then Lockdown Policy, for example.

Or let's use the last one on the list which is Scripts Policy and I can go ahead and get scripts policy.

I can also assign this to a variable.

So if I go $gpo equals Get -GPO -Name Scripts Policy then now $gpo represents this GPO, and I can do some modification on it.

So, for example, you see the GpoStatus property here, and remember I mentioned there is no direct cmndlet for modifying status, but what if I do this? So what if I said, dollar, dollar, $gpo, GPOStatus.

It returns all settings enabled.

Now I happen to know that that's an enumeration that lists, or that, that contains all the possible values.

So if I were to say dollar GPOStatus equals 0, and then go back and say dollar GPSStatus.

All settings are disabled now.

And that's actually been changed in the GPO permanently.

You'll see here now, that the property or the status on the GPO is set to disabled.

So, I can make modifications to certain properties.

Not all properties are modifiable, but in that one, in that particular case it was, so I was able to make a settings, or status change to that GPO using PowerShell.

So there's a lot of flexibility in what you get with the PowerShell cmndlets.

And I'm going to talk more about those.

And I, I wanted to just kind of introduce you to the module, get you familiar with some of the basics.

And the -Get -GPO cmndlet is probably the most basic one you'll start with.

---

## Basic GPO Management With PowerShell

So let's look at some basic GPO Management tasks that can be accomplished with PowerShell.

So, let's start with creating, renaming, and deleting, and also counting GPOs.

So, to create a GPO, it's really simple, you just type New-GPO give it a name and you're good.

Now for renaming a GPO, you just use the Rename-GPO cmndlet, you give it the existing name using the -Name parameter and the -TargetName, meaning the name you want to rename it to.

In this case, Marketing Lockdown Policy.

And you're good to go, it makes the change.

I also want to note that, the GPO names in this case are not case sensitive, so you can use lower or upper or any combination as long as it matches the case insensitive name value, you're good to go.

So don't worry about either GPO name case or even parameter case.

I, I try to capitalized my parameters here, but I haven't done it universally and it actually doesn't affect the behavior one bit.

So to delete a GPO, you just call the Remove-GPO cmndlet with the name of the GPO.

And then finally, if I wanted to get a count of all GPOs in my domain, I can simply type Get-GPO -All.

Put that in parentheses, and that acts as an, an expression.

And then get the count property on the result of that.

So what that's telling the cmndlet to do is go get all the GPOs and I don't really need to see that big display of each GPO in detail.

I just need to know how many there are.

And that works just fine and gets you, quickly gets you to account for all GPOs in the domain.

Now to modify permissions or delegation on a GPO, it's a little more complicated, but not too bad.

To retrieve the permissions, you can just type Get-GPPermission.

Give it the name of the GPO.

And if there's a particular permission you're interested in, you would have to use the target name, which is the group or user name that you're interested in the permission on.

Or you can use the -all parameter.

It will return all the permissions on the GPO.

If I wanted to say, add the Sales Admins group with edit rights to a particular GPO, I can use Set-GPPermission.

I use the name parameter to specify the GPO I'm interested in.

I set the PermissionLevel, which is a parameter that has a number of possible values, in this case at GpoEdit.

The -TargetName is the group that I'm interested in, and the -TargetType is the thing that it is.

So it could be a group, a computer, or a user as the possible values.

Now if I wanted to remove that Sales Admin group.

I could go in and set the permissions for that same GPO with the permission level of None, for that group, which it, with the -TargetType of Group.

So, pretty much the same command.

The only difference is that -PermissionLevel parameter goes from GpoEdit to None.

So now for Linking GPOs.

It's really simple.

Call the New-GPLink parameter, or, I'm sorry, cmdlet.

Give the name of the GPO you want to to link.

The -Target parameter takes the distinguished name of the container.

It could be a domain, a site, or an OU.

In this case, I'm linking under the Users OU, under the Marketing OU in the r3test.tld domain.

I have to tell it that I want the link to be enabled and I have to give it an order.

Remember, you can have multiple GPOs linked on a particular container, like an OU, so you have to tell it in what order you want this link to appear.

So in this case, I'm going to make it the very first link in the order, the highest priority.

To set that same link to Enforced, I can use the Set-GPLink parameter,or, I'm sorry, cmdlet, with the name of the GPO, the Target OU, and then the Enforced parameter set to Yes.

And then to remove that link all together, all I have to do is specify the GPO name, and the target DN.

For backing up, restoring, or importing a GPO.

Backup is really easy, give it the name of the cmdlet, Backup-GPO, name of the GPO you want to back up, the path where your backup files are located, a comment, which is optional.

And you're good to go, it'll create that backup.

Now, to restore the GPO, it's a little more, little bit more confusing.

You have to use the Restore-GPO cmdlet.

You give it a BackupId, which is not the name of the, or not the ID of the GPO, not the GUID of the GPO.

It's the GUID of the folder that gets created in the backup folder when you created that backup in the first place.

And I'll show you that in a little bit.

And then the -Path is the path to where the backup files reside.

Now to import a backup into a new GPO.

So this, in this case I've got a backup.

That same backup, I want to use it.

This time I want to create a new GPO called Imported Policy.

And the Import-GPO cmdlet gives me the option of specifying the BackupID, the path where the backup resides, the target name of the GPO, in this case, it's a new GPO.

So I'm going to use the -CreateIfNeeded parameter to create that GPO before it actually does the import.

If I didn't have that parameter in there, it would look for an existing GPO name called Imported Policy and import the settings right into there.

So let's spend some time and now and look at what these cmdlets look like in practice.

---

## Basic GPO Management With PowerShell

So let's run through some of these cmdlets that I just used in the past slides.

So let's go ahead an create a new GPO.

And again, I'm going to use the New-GPO cmdlet with a name of Test Policy.

And what you see is it returns information about the GPO, as it's being created.

So, it got the name.

It's got the domain name, the owner, AD version of course is 0.

If I come back into GPMC, go up and refresh my Group Policy Objects container, you'll see that, sure enough, it's created that GPO.

Now, what I want to do is rename this GPO, using the rename-GPO cmdlet.

So let's go ahead and paste that command in.

So we'll do rename-GPO -Name Test Policy.

And I'm going to give it a new -TargetName of Marketing Lockdown Policy.

And there we go.

I've just renamed it.

So if I again, come back into GPMC, if I hit F5, you'll notice that it just got switched to Marketing Lockdown.

And, after all that, I can go ahead and say Remove-GPO Marketing Lockdown give it the -Name parameter just to keep everything clear, Marketing Lockdown Policy.

And it deletes the GPO, no fuss, no muss.

Again come up to GPMC, hit Refresh and it's gone.

Now, remember, I said that you could easily get a count of all GPOs using the Get-GPO cmdlet with the -All parameter, and passing it as an expression to the Count property? And there I got, I've got 28 GPOs in this domain.

So all good.

So now what I want to do is a want to get some information about some existing GPOs, I've got this Lockdown Policy, Lockdown GPO, and I'm going to go ahead and get the permissions on this.

So I'm going to go ahead and get the permissions on the GPO.

So, Get-GPPermission of the -Name Lockdown Policy and I'm going to say I want -all permissions on the GPO.

So now what it did is it returned four objects for each different permission.

Here's Authenticated Users with the GpoApply permission, that's basically that security filter that lets that authenticated users group pos, process this policy.

Now what I want to do is go ahead and set the -GPPermissions.

Now I'm going to go ahead and clear the screen here, so that I can get a fresh canvas.

Now what I want to do is set permissions on that Lockdown Policy, so I'm going to go ahead and copy that command in.

And what I'm going to do is set permissions on the Lockdown Policy, -PermissionLevel is going to be GP, oops.

Let me get back to my parameter here.

And I'll show you the possible per, permissions.

So I can have Gpoapply, GpoCustom, I'm going to use GpoEdit, -TargetName is the Sales Admins OU or group and the -TargetType is Group.

So now, if I do a Get-Permission on the Lockdown Policy, Get-GPPermission on -Name Lockdown Policy.

Use the -all parameter, and now I've got that Sales Admins group that's been added.

So now let's go ahead and clear the screen again and let's go ahead and do a linking of our Lockdown Policy to an OU.

So we've got our Users Marketing OU and I've specified the Lockdown GPO with the New-GPLink cmdlet.

And I want to tell it that the -LinkEnabled is Yes and the -Order is number 1.

So let's go ahead and issue that command.

And it comes back and tells me that it did it.

And if I come into the Marketing Users OU up here.

And hit Refresh.

You'll see I've got my Lockdown Policy all linked up there.

Now if I wanted to, for example, change that link to be enforced, I can come back into PowerShell and paste in the Set link -GPLink cmdlet.

Again with the -Target of the OU.

And -Enforced set to, you can't see it here but it's set to Yes.

And if I hit Enter and comeback to GPMC and hit Refresh, you'll note the little lock symbol that just showed up there, so I've been able to do that refresh.

Now let's look at backing up GPOs.

Again, we've got lots of capabilities within PowerShell to do this.

The Backup-GPO cmdlet.

Give it the GPO name, give it the -Path to the, to the backup folder, and a -Comment, in this case, Lockdown PowerShell, Lockdown Policy PowerShell Backup.

And it takes a little bit to run, but then it comes back, tells me that it's made the backup.

Now this is the backup ID that was created.

And if I go into my Backup folder, let me just go back up here, to My GPOBackups folder, you'll see that this 76BEA corresponds to the folder name, and that's the, the actual backup ID.

So when it comes time to do a restore or an import and I have to feed it the backup ID.

You'll see, if I paste in my -Restore command.

That I've got a BackupId.

This is actually a different backup, but if I come up here, I can get the BackupId that I just created and go ahead and put it into this command.

So let me go ahead and clear out that one and paste in that new BackupId.

And I can do the restore, and it restores me back to the GPO settings that were in there from this backup.

So, again, I'm using the -BackupId as the thing that I'm keying on for the restore.

And this is, you know, just a sampling of the things that you can do, a pretty broad sampling of the things you can do in the PowerShell Group Policy module.

Next I want to look at the capabilities of some VBScript sample scripts that are available from Microsoft.

---

## Leveraging VBScript With GPMC

Now I'm going to finish up this module and this course, in fact, with a little bit of a blast from the past and talk about leveraging VBScript, which is sort of the old Microsoft scripting way in GPMC, and how it's still relevant.

So.

Why VBScript when PowerShell is around now? Well, there are some functions that are not accessible via PowerShell, or via the commandlets I should say that Microsoft provides.

And so there's actually some sample scripts that Microsoft has provided a bit around forever that gives you some of these capabilities in canned functionality.

And I've got the download link here, it's on the Microsoft Download site.

If you just search on GPMC sample scripts, you'll get there, but this link also provides that.

And it provides a set of some neat functionality.

And there's some scripts in there that are particularly worth noting.

The first pair of scripts are called CreateXMLFromEnvironment.wsf and CreateEnvironmentFromXML, and these are companion scripts that allow you to, believe it or not, take an entire active directory structure including users, groups, GPOs, even computer accounts and essentially clone that into a, for example, test domain or test forest, that's got the same structure.

So it allows you to mirror or create a copy of your existing forest and domain with all of its AD structure, and groups, and GPOs, et cetera in this test environment.

And it's a really beautiful script if you ask me for doing this, and I haven't seen it replicated at all in group, in, PowerShell yet.

A couple of other scripts that are really handy, FindorphanedGPOsInSYSVOL.wsf, that lets you see if there are GPOs lying around that have been corrupted.

So, for example, if a GPO has been deleted.

But the SYSVOL portion of it, for whatever reason, wasn't deleted, only the AD portion was deleted, this script will find those orphaned pieces of the GPOs in SYSVOL.

Another one is find GPOs by policy extension which is a nice little script for getting a listing of all the different policy areas that are implemented across your GPOs.

So let's take a quick look, at how these scripts work and then we'll finish up the module and finish up the course with this discussion of Group Policy scripting.

---

## Leveraging VBScript With GPMC

Okay, so I've gone ahead and installed the GPMC sample scripts that I mentioned in the previous slides.

And you'll see here that they get installed into the Program Files (x86) > Microsoft Group Policy > GPMC Sample Scripts folder.

And here are all of the scripts that are available, so there's a bunch of them.

And, I can run these scripts by bringing up a command prompt and calling cscript against the script name.

So, cscript is the, if you'll remember, that's the, command line version of the VBScript interpreter.

So if I say, cscript FindGPOsByPolicyExtension, and I can give it an extension name like Registry, which is the name of the admin templatex extension, and it shows me all of the GPOs with registry policies defined for users and for computers, which is kind of a handy piece of information to have.

Now remember that create XML from environment script.

Let's take a quick look at that.

I'm going to go ahead and clear my screen, and say cscript CreateXMLFromEnvironment.

And here are all of the options that I can feed into this.

There's an output file which is the XML file that gets created when you run this.

There's a template path which is the location to store the GPO backups so it creates a backup of GPOs.

The domain that I'm interested in specifying or capturing the DC I want to use.

A starting OU if I want to have just capture a part of the domain at an OU level, I can have it do that.

I can say ExcludePermissions, IncludeAllGroups and IncludeUsers.

So I can use these options to essentially create a XML file and GPMC backups of all my GPOs that represents this environment.

And then I can take those files and put them on another machine, on another domain controller that's a completely empty brand new domain.

And I can use those to essentially restore my environment as it exists today into that new domain, so lots of cool features in this GPMC sample scripts.

Just point out a few others that are kind of handy that don't have necessarily direct corollaries in the in the PowerShell commandlets.

This DumpSOMInfo, SOM means scope of management and this refers to a site, domain, or an OU.

So this will give me, for giving SOM that I specify information about GPOs that are linked to it, I can use this FindDisabledGPOs script.

Let's go ahead and run that to see if we have any GPOs that are disabled in the domain.

So cscript FindDisabledGPOs.

And here it says that, there are one GPO with, that are completely disabled and it's called the Scripts Policy.

And remember, I had set the status on the Scripts Policy using PowerShell to disabled and sure enough we found it.

So lots of cool things that I can do with the GP, the VB scripts that I can't do directly with the PowerShell scripts.

And there's a nice scripting read me that has some documentation about this in here if you need to check that out.

So, I would highly recommend using the VBScripts to sort of supplement your arsenal as you get more familiar with PowerShell.

And there are some things like the create environment from XML and create XML from environment scripts that you just can't find in PowerShell.

One last script that I really like to show is the ListSOMPolicyTree script, so that's this guy here and it's kind of a good script to end on.

So if I go ahead and say cscript ListSOMPolicyTree, what it'll do, is it'll draw a sort of pictorial representation of my domain and show all the GPOs that are linked to it.

So here's the domain level, here's the OU domain controllers, with the domain, default domain controllers policy.

And here's the marketing OU with the four GPOs that I have linked to it.

This is a really great, kind of, I find this script to be absolutely fantastic for doing just basic documentation of the environment, visual documentation.

So again, VBScript may be old, these scripts are from 2007 but they still have a lot of value in your group policy environment.

---

## Summary

Let's summarize what we've learned in this module.

So, the Microsoft Group Policy Module from PowerShell, provides a lot of functionality for group policy management.

The big notable exception is support for reading and writing most of the GPO setting areas, so, there's really zero support for making settings changes outside of things like the, the GP preferences registry extension and admin templates.

The commandlets let you do, other than the settings, but you really do a lot of operations against GPOs including creating, deleting, renaming, permissioning, linking, backing up and restoring, and importing.

And also doing some reporting against them so you can do GPO settings reports, you can do result instead of policy reports.

All that's available in the module.

But, what we did learn is that VBScript still has a role to play here at the courtesy of the Microsoft GPMC sample scripts that are still out there from 2007.

And I showed you the create environment from XML and create XML from environment tandem of scripts that have a really useful function, which is the ability to capture the AD structure, user objects, groups, and GPOs from one domain, let's say your production domain.

And essentially use them to create a mirrored domain in a different domain altogether.

So essentially, using all the capabilities that we've talked about in this course, around backup, and restore, and import.

And in addition to a bunch of AD functionality that we haven't talked about, this script lets you, basically create a mirrored environment.

And I also showed you the ListSOMPolicyTree script, which is a really cool way of visually representing which GPOs are linked to which containers in your AD.

So that pretty much wraps up this module and this course.

And I hope you've enjoyed it as much as I have, and enjoyed learning about group policy, and I think that group policy is one of those journeys that never ends.

I've been doing it for 14,15 years and I'm still learning each and everyday that I do something in group policy so, hopefully this has wetted your appetite for further learning and further exploration, and to take full advantage of group policy in your environment.

---

