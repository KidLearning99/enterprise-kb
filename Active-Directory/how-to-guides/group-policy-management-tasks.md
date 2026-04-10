# Group Policy Management Tasks

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Group Policy Management Tasks |
| Type | Automated Transcript Synthesis |

## GPO Backup and Restoral

Okay.

In this module, I'm going to talk about some of the group policy management tasks that you can perform against your group policy environment.

And really most of these are going to come out of the GPMC.

And what I wanted to do now that we've talked about kind of basics and some scenarios around using group policy, I wanted to dive into more aspects around managements and troubleshooting in this last few modules.

So let's go ahead and dig in.

So, there's a set of operations that you're going to want to perform against your group policy objects on an ongoing basis.

And, and this goes beyond just the basic stuff that I've talked about in creating new GPOs and editing them and linking them, and all that sort of stuff.

So there's Group Policy Backup and Restore, which is kind of one of those hygiene items that you always want to be doing within your environment.

Can't tell you how many times I've seen situations where users weren't backing up their group policy objects, and then got into a pinch where they made a change that they couldn't restore.

Or they deleted a GPO, and without that backup you're just kind of out of the water.

There, there is a way if you backed up your active directory, there is a way to get at GPO backups through a system state restore but it's really painful.

The other aspect that I want to talk about is delegation of the various group policy management tasks.

And group policy has a reasonably rich delegation model.

So I'm going to go into that and talk a little bit about how that works.

And then finally group policy import.

This is a little bit different from the backup and restore function, and I'll talk about how it's different, but this gives you a number of other scenarios that you can use to take advantage of your backups to bring in GPOs from other domains or restore other GPOs that have been deleted.

So let's look at backup and restoral to start with.

So GPO backups can be created in GPMC.

And in the last module in this course I'm going to talk about using PowerShell to manage group policy.

And I'll get into the way you can do backups using PowerShell.

But for the time being we're going to talk about doing it in the context of GPMC.

And you can do backup of a single GPO or there's a function where you can do a backup of all GPOs and I typically recommend folks if you haven't backed up your GPOs before to run the all GPO backup first and get a baseline.

And then as you make changes over time you can do one off backups of GPOs that change.

GPOs are used to roll back.

GPO backups are used for rolling back existing GPOs, so that's the use case.

And there's a Restore function that you can get access to, so if you make a change to a GPO and you want to roll back that change, you can restore from a backup.

Backups are not there to restore a deleted GPO.

So that you might think that your, you know you make a backup of a GPO and you delete it, I can restore it.

Well, the restore function, the backup is good for getting back to a known good state, but the restore function is not useable in the deleted GPO scenario.

So that's the job of the import function, and I'll talk about that.

Backups themselves are stored in the file system and I'll, I'll show you what that looks like and it's underneath whatever folder you select to store the backups.

You can just specify a UNC path or a local path on your workstation where you, where you're doing GPMC management.

I typically recommend having a backup of your backups.

So make sure that your backups are stored on some location that gets backed up so that you don't make your backups and then have your hard drive crash.

And, and no longer have access to them.

And then they can be viewed and deleted from the Manage Backups dialog.

Which is what I have in this screen shot here.

And this is just showing you some backups that I did on this downloads folder under my user profile.

And I've got, you know, three different GPOs that have been backed up.

And the folder where the backups are kept is shown at the top and you can change that folder if you keep backups in different locations.

The list of all the backed up GPOs are shown with their timestamp and their ID and the name of the GPO.

And you can, you can put a description on a backup when you make the backup, so that's what's shown here in that description column.

You can view the settings in a backup, so you can click on a backup and click the view settings button, and it'll actually show you the settings that have been backed up.

So that's a good way of seeing, you know, what the backup is that you're potentially going to restore.

You can delete a backup, so you can use this dialogue if you have backups that are no longer valid or they're too old you can delete them from here.

And you can use the restore button to actually perform the restore against your live GPO.

You can also do this from the GPO itself, but from the manage backups UI you're able to do that.

---

## GPO Backup and Restoral

So now, what I want to do is demo the basic backup and restore operation.

So under Group Policy Objects, if I right-click the Group Policy Objects node, you'll notice I have the Backup All option.

And if I run that, it'll give me the option of setting a location and giving it a description.

And then it'll go ahead and back up all of those GPOs with that same description.

But let's for the sake of this demo, let's go ahead and go into one of our GPOs, and we've got this GPO here.

It's the kiosk user settings GPO that I used in a previous demo.

What I want to do is go ahead and back up this GPO.

So I'm going to go ahead and back it up, and I'll say that this is the baseline kiosk user backup.

And it's going to go ahead and back it up to that folder.

Now once the backup is completed, I'm going to go in and use GPO Editor to make a change to the backup.

So if I go in under User Configuration > Admin Templates > Start Menu and Taskbar and look and see.

I have this Lock the Taskbar setting enabled.

And I have Prevent Access to Shutdown, Restart, Sleep and Hibernate enabled.

I'm going to go ahead and set this to Disabled.

Let's say that was my change.

And I made an inadvertent change that I didn't want.

So now the policy has been set to disabled.

And if I go ahead and refresh the report and bring that up, you'll see that it's now set to disabled.

So what I can do now is come in and restore from backup to get back to the way I want it to be, and there's a couple ways I can do that.

One is I can come, right-click the GPO and say Restore from Backup, the other way is I can right-click the Group Policy Objects container and bring up my Manage Backups dialogue.

And, what you'll see here, is now it's showing me that kiosk user settings baseline.

I can come in here and view those settings.

And it's going to give me an option of how I want to view it.

It views it as an HTML file, so I'm going to go ahead and chose IE to view the settings.

And it, essentially what we're seeing here is an HTML settings report of the GPO, as you might see it in GPMC.

And once that comes up, I can see that, within the backup, that remove and prevent access to shutdown, restart, sleep, and hibernate is still enabled.

So I'm viewing the backup now.

And I'm going to say okay that's the backup I want, I want to go ahead and restore.

And it'll ask me do you want to restore the selected backup.

Now it knows the GPO from which it was the bey, the backup was created.

So it's going to use that GPO to restore into.

So it's going to say okay.

Says it succeeded on the restore.

If I go ahead and close this.

Come back to my Kiosk User Settings GPO.

Click on Refresh to make sure I get the most recent report.

And you come in under the settings and you'll see that this is set back to enabled.

So, essentially, I have been able to, using the backup, I've been able to roll back the change that I made on this GPO which is a really handy feature to have.

And, again, I highly recommend that you use this feature to back up your GPOs on a regular basis, whenever you make a change to a GPO, just come on in here and say Back Up before you make the change, and it'll save you a lot of grief in the long run in terms of managing that process.

You can also use Powershell, which I'll show you again in a later module to automate backups of GPOs.

So, hopefully, that helps kind of give an overview of the backup and restore process.

---

## Group Policy Delegation

So, now I want to talk about Group Policy Delegation.

And more specifically, group policy management delegation because we're going to talk about more than just delegating access to GPOs here.

So, in terms of group policy management, what actually can be delegated? Well, there's actually quite a bit.

In the GPMC, you can get access to quite a granular security model.

So GPO creation can be delegated, GPO editing, of course, GPO linking, so who can link to which OUs sites and domains.

The ability to perform result instead of policy modeling which is something we haven't talked about yet.

We've talked about "RSOP" logging which is also called group policy results.

But both of those operations can be delegated, so you can control who can do which of those operations.

WMI Filter Creation and Editing.

And Starter GPO Creation.

And, I haven't yet introduced the concept of Starter GPOs, which I'm about to do in this module.

So, we'll wait on that section to talk a little bit more about what a Starter GPO is.

But these are all the operations that you can delegate.

Now, in terms of where you do this, within the GPMC, most of this stuff is accessible through the GPMC.

In fact, all of it that I'm going to talk about is accessible through the GPMC.

And what I'll say is that, when it comes to delegating, doing delegation operations against group policy stuff, your best bet is to do it through the GPMC rather than trying to figure out how to do it against native Active Directory objects or permissions.

And the reason I say that is because I've seen some folks try to work around the GPMC model by delegating under the covers using active directory directly, and GP doesn't always respond well to that unless you've done it really consistently across the board.

So, using GPMC to do delegation is your best bet.

And in this case, if I right click on the Group Policy Objects node, I can delegate the right to create GPOs in this domain.

And as you'll note here, the domain admins group, a special group called Group Policy Creator-owners.

And the local system account have this right today, or have this right by default in the domain.

So these are the groups that can create GPOs by default.

So with respect to delegating the linking of GPO's to containers, or RSOP Modeling and Logging on computers in those containers.

If you right-click on a container, as you can see in this diagram, I'm right-clicking on the client's OU and hitting the delegation tab, and I get a drop down list of things that I can delegate.

And what this means is for each of these permissions link GPO's perform group policy modeling analysis and read results data.

I can delegate to different groups of folks.

So I can delegate who can link GPOs to this client's OU.

I can delegate who can perform group policy modeling against computers in this OU or who can perform group policy results of group policy logging to computers in this OU.

So you can set this either at the OU level or at the domain level, or even at the site level, you can set each of these rights for linking group policy modeling in group policy results.

In terms of delegating who can edit GPO's, once a GPO has been created, you can click the GPO under the group policy objects container.

And on the Delegation tab, you can control who can read the GPO, who can edit settings, delete or modify security.

And that permission is actually granting the user the ability to modify the delegation of the GPO itself or you can just grant the edit settings permission, in which case they'll be able to edit the GPO, but they won't be able to delete or modify security on the GPO.

Now with W my filters, if you click on the W my filters container, you'll have the delegation tab available to you, and you can delegate who can create and edit GOP, or sorry, WMI filters in this domain, and similarly with starter GPOs.

If you've enabled starter GPOs in your domain, you can use the delegation tab to enable who can create starter GPOs in the environment.

So, you get all of these delegation capabilities within GPMC, and the ability to delegate at a very fine grain level who can create, edit, link, re-permission, create W my filters; all sorts of activities related to Group Policy Management.

So, let's dive in and take a look at some of this in my test system here.

---

## Group Policy Delegation

So, let's look at a scenario where we want to delegate the ability for Joe Sales in the sales group to temporarily be a Group Policy admin guy, and Joe Sales needs to be able to create a GPO and link it to the Sale's Users OU, which you see here in GPMC.

So, right now, if I right-click on the Users OU as Joe Sales, you'll see all of these options are grayed out.

I can't do pretty much anything against this OU.

And if I try to come down under the Group Policy Objects Container, and let's say I want to create a new GPO.

If I go ahead and say new, and I'm going to say, Joe's Sales GPO and click OK, I get access denied.

So at this point, Joe's Sales can't really do anything from a group policy admin perspective.

So what I'm going to do is I'm going to come over to my GP management console running on my active directory domain controller, and, I'm going to go ahead and delegate Joe Sales the ability to create GPO's.

Now, what I actually prefer to do is to delegate it to a group rather than an individual user.

So, what I'm going to do instead of that is I'm going to come down here and I'm going to create a new group, and I'm going to call this group, Sales Admins.

And once that group's created, I've got a global group here I've created.

I'm going to go ahead and add.

I'm going to add Joe's sales to the sales admin group.

So I'm going to go ahead under members and say, add Joe to sales admins.

And I'm going to come back to GPMC and I'm going to say, add a delegation on the group policy objects container.

So that sales admins can create GPOs in the domain.

And so now, sales admins can create GPOs in this domain.

And if I come down to the sales OU, I want to let sales admins link GPOs to the user's OU, under the sales OU.

So if I come over to delegation here.

I select the link, and I add sales admins.

Go ahead and Check Names on that, and grab the right group.

And I can say, do I want it on this container and all child containers? Well, I don't have any child containers under sales or under users, but I'm going to go ahead and select this container only, just to make sure that it stays that way.

So now the sales admin's members can link GPOs to the Users container, and they can create GPOs.

So let's go back over to our client, and since I added Joe Sales to a new group, I'm going to have to log out and log back in, in order for that Joe Sales membership to take effect, so I'm going to do that right now.

And now I'm logging back in as Joe Sales.

And this time I'll get my group membership in sales admin for my new session.

Okay, I'm going to go ahead and run GPMC.

And let's go ahead and go down to the Group Policy Objects Container.

And I'm going to say, New GPO.

So I'm going to go ahead and create a new GPO called Sales Admins GPO.

And it let me create that GPO.

So there it is.

And if I look at the delegation on the GPO, you'll see that Joe Sales, not the sales admin group, but Joe Sales has been given full control over that GPO.

So what happened there is that Joe Sales, as a member of the Sales Admin group created the GPO, and his individual account has been given delegation to the GPO to make changes to it.

So, even though, there might be another, Sales Admin user, that user is not going to have access to the GPO that Joe Sales created.

They're not going to be able to make changes to it.

That's an important point to note about delegating access.

Then when I go in under Sales > Users, and I'm going to right-click, and you'll notice that now these options are not grayed out.

And I can go ahead and say, link an existing GPO.

And I can link the Sales Admins GPO and I'm good to go.

Now one thing to note is, if I wanted to try to link to the Marketing OU I still can't do it.

So, that's not available to me.

Just because I have access to the Sales OU, doesn't mean I have access to the Marketing OU.

And in fact, if I go up to the parent level of the Sales user OU, I can't link there as well.

So, only this delegated container can I link to, using this permission.

One other thing I want to mention about the permissioning.

Remember that I mentioned that when you're on a container you can delegate Group Policy modeling and Group Policy results as well.

The Group Policy Results option actually does two things.

The first thing it does is it lets you run as a user, it lets you run Group Policy results against computers that are in this OU.

The other thing it does interestingly enough, is it lets you do remote GP update.

I introduced this concept in an earlier version in an earlier module.

Remote Group Policy Update is a feature on Windows 8.X and Server 2012 R2 and Server 2012.

And the only way to delegate, in other words, control who can do Group Policy Update, is to grant them this seemingly unrelated permission of Group Policy, Read Group Policy Results.

So by granting this right you grant the user both the ability to run RSOP against the remote system, and also to do a remote GP update against that remote system.

So just something to keep in mind.

Not well-documented at all, but something that's important to know if you're planning to roll out the ability to do remote Group Policy updates.

---

## GPO Import Tasks

So now I want to shift gears and talk about GPO importing.

So as I mentioned earlier, GPO importing is a little different from the backup-and-restore scenario.

GPO importing does use GPMC backups, but it works in a little different way.

So, it's good for primarily recovering deleted GPOs.

So if a GPO gets deleted completely, you can create a new GPO and import a backup of the previous GPO into it.

Or you can import settings from other domains and forests or import GPOs from other domains and forests.

So if you have, for example, a test domain or a test forest, and you want to test out your GPOs there first and then import them into your production forest, that's the scenario where import comes in handy.

And import can use this feature called Migration Tables.

Migration Tables have been around since the beginning of Group Policy, and they're designed to convert certain types of data within Group Policy objects as they're being imported into the new domain.

So the scenario is you have for example, restricted groups policy that is using groups that are defined in the source domain.

Maybe it's your test domain.

And you want to convert them to the same or a different named group in the production domain, and that's the purpose of Migration Tables.

So they can convert user names.

They can convert computer names.

They can convert domain groups.

So domain local, global, and universal groups, and UNC paths.

And UNC paths, typically where you'll find those is in folder redirection policy or in software installation policy.

Where you've got a reference to, in the case of folder redirection, a path where you want to redirect your folders, or in the case of software installation, the path to the MSI file.

So in both those cases, you can redirect those UNC paths or remap them as you're importing the backed up GPO.

And the final use case here is what are called Free Text or SIDs, and this is where you might have entered, let's say in restricted groups or user rights assignment policy, you might have entered in, by hand the name of a group or a user, and it doesn't get resolved to a SID per se.

And it's entered into the Group Policy Settings field as just the name and the textual representation of the name or the SID.

And in that case, you can also convert those values as they're imported into the new domain.

Now, migration tables only support policy settings.

There's no support for any of this kind of remapping of principals and UNCs in Group Policy Preferences.

So if you're trying to move Group Policy Preferences from one domain to the other, it's completely useless for that unfortunately and Microsoft doesn't really have a good solution there for that scenario.

So, this is kind of what a migration table looks like, and as you'll see in here you have a source name.

In this case there's a source of MarketingUsers@cpandel.com and it's a domain global group and it's being mapped by relative name into the destination.

And what that means is, we're assuming that there's a marketing users in the destination domain, and we're going to remap it to MarketingUsers@STMlab.test, or something like that.

So that basically lets you control how the name gets mapped as it goes into the destination domain.

So let's dig in and take a look at how this works.

I'm going to go ahead and do a migration, so you can see what this looks like.

---

## GPO Import Tasks

Okay, I'm going to look at two scenarios that use the import feature.

The first is the scenario of the deleted GPO.

You'll remember from earlier in this module, I created this Kiosk User Settings GPO.

And made a backup of it and showed you how you can use backup and restore to roll back the settings in the GPO once they've changed.

I'm going to go ahead and delete the GPO now, so Kiosk user settings is going to go away.

And I'm going to come back in here and I'm going to create a new GPO because, hey, I really want Kiosk user settings back in my domain.

So I'm going to go ahead and create a new GPO called Kiosk User Settings and I'm going to click OK.

And now that that new GPO is there, you'll see it's got no version number which means there's no settings in it at all.

I'm going to go ahead and say import settings.

And I'm going to run through the import wizard.

And it lets me, if I want to, I can back up this GPO.

Because essentially, what import is going to do is completely wipe out the existing GPO and rewrite it with the settings that are in the backup.

Well, since there's no settings in this GPO because I just created it, I don't need to back it up.

I can tell it the backup folder where my backup exists.

And if I go into the backup folder, I'll see all of my backups.

And here's my Kiosk User Settings backup.

So now, I'm going to go ahead and click Next, and lo and behold my GPO is now restored.

You'll notice the user version and the computer version incremented.

So I've got, I can prove that my GPO has been restored.

What's interesting is that I don't get the original GUID of the GPO, or unique ID of the GPO back.

I still have the GUID of the new GPO.

The other thing to note is that when you delete a GPO.

Any, containers, OU's, domains, or sites that that GPO is linked to, those links are removed.

So, even though I've recreated the GPO and restored it from a backup using import, none of the links that were in place for this GPO are there.

So you're going to have to recreate those links manually.

But let's go in and look at the settings report.

And you'll see that, sure enough, all of my settings that were in that original Kiosk User Settings GPO are back.

So I've essentially used import to recreate this policy.

And that's a key feature of the import function.

Now what I want to do is show you the use of a import for bringing a GPO from one domain to the other.

So I'm going to go ahead and create a new GPO and I'm going to call it User Rights Policy.

And I'll show you why in a second.

This User Rights Policy is going to get created.

Now what I want to do is, I want to go up to the Group Policy Objects container, right-click and say Open Migration Table Editor.

And now I've got a new migration table.

Remember I talked about migration tables as the thing that you can use to map basically, security principles, and UNC paths.

From a backup, a GPO backup, or even a live GPO from one domain to the other.

And what I'm going to do is, I'm going to say Populate from Backup.

And I'm going to point it at a GPO that I've got from a foreign domain.

We'll call this my test domain at cpanel.com.

I'm going to go ahead and point at that GPO.

And I'm going to say, OK.

And now, what it does is it identifies all of the security principals in that source GPO, and lets me map them.

And actually, what I want to do is I want to do that again, and I want to show you some other option.

You'll see this option down here that says During scan include security principles from the DACL on the GPO.

The DACL is the security filter on the GPO.

So that's the delegation that's in place on that GPO.

And you can translate or migrate those security principles.

Just as easily as those groups or security principles that exist within settings inside the GPO.

So I'm going to go ahead and include those.

And you'll notice that my list suddenly got a lot longer.

So and you'll notice that now groups like Enterprise and Domain Admins, GPO Admins.

All have come across as being part of that backup.

Now, what I want to do, is, I want to map these for the destination domain.

So I'm going to right-click in this destination domain cell, and I have three options.

I can either say, No Destination, which means that it's not, it's just going to drop that security principle.

Wherever it exists, when it's bringing that GPO backup over.

Map by Relative Name, which means leave out the domain part of that.

And just translate from, in this case enterpriseadmins@cpandel.com to enterpriseadmins@r2test.tld which is what I'm going to do.

And I can do that the same for the domain admins group because it exists in my destination domain.

And, same as source for everyone.

For marketing users, I'm going to go ahead and say Map by Relative Name because I'll have a marketing users name in my destination domain.

Now I don't have a GPO admin's group in my destination domain, so I'm going to say, No Destination.

In other words, drop that group from the GPO as I bring it over.

Administrators, same as source.

Sales users, I'm going to say, Map by Relative Name, and Authenticated Users, Same as Source.

So now I've got my migration table.

I'm going to go ahead and save that migration table.

And I'll go ahead and save it as User Rights, and it gets saved with a .mig table extension, saved it in my Documents folder.

And now let's go back to that user rights GPO that I just created.

And I'm going to say Import.

Click Next, click Next, again, click Next into the backup.

And go down to my user rights backup.

And I can, you know, view settings on this to see what settings are in this GPO.

And you'll notice, I have a bunch of user rights assignments and there are those groups that you saw the migration table pick up on.

And if I go into delegation you'll see this is why it picked up on domain admins, enterprise admins, GPO admins, etc.

So I've got that GPO, that back up that I'm interested in.

I'm going to click Next.

And it's going to scan for security principles.

And you'll that it says the backup references to security principles and or UNC paths, specify how these should transfer.

So I'm going to click Next, and I'm going to say, use the migration table.

And it found my migration table that I just created.

You can browse to another one if you want.

It also gives you the option to say use the migration table exclusively.

So if any security principles or UNC paths are not found in the migration table do not perform the import.

In other words, just stop the import.

We don't want to do that, we're just going to go ahead and drop those principals that we don't map.

And I'm going to go ahead and click Finish.

And it succeeded in bringing in that new GPO or that backup from my other domain into the User Rights policy.

And now you'll see under Local Rights.

User, local policies, user rights assignment that it mapped the Cpandle marketing users and Cpandle sales users to the R2TEST domain.

So it made that change.

And if I look at delegation, you'll see that it picked up domain admins and enterprise admins and authenticated users, just fine as well.

So all of the security principles have essentially been migrated from the test domain to this new production domain.

---

## Starter GPOs

The final, group policy management concept I want to talk about in this module is the starter GPO.

So what is a starter GPO? Well, it's a sort of a reusable model or , Gold image, if you will, of Admin Template Settings.

So, it does only support Admin Templates and it provides this ability to create commonly used Admin Templates that can be used over and over again in new GPOs that you create.

So you start with a starter GPO, and Microsoft even provides some kind of default ones out of the box that might be sort of commonly used configurations.

And then, you can edit it just like you would edit a regular GPO using the group policy editor.

But it's only exposing you to Admin Templates, so you can't create starter GPOs of security settings or folder redirection, software installation, any of the group policy preferences.

It's really just limited to that Admin Template's policy path.

And, again, you can, once you've created these sort of templates or golden images of GPOs, you can then go in and create a new GPO from a starter GPO.

And what essentially happens there is it takes the settings that you've defined in that starter GPO.

And it's very simply just copy them into the new GPO so that when you open up GP editor on the new GPO, all of those Admin Template Settings that you defined in the starter GPO are already there.

And then you can go and build more settings into that GPO.

But it essentially let's you sort of define a beginning point for a standard, maybe corporate standard, for Admin Templates.

You know, to be honest, I don't see a lot of people ending up using these, but there have been scenarios where I've seen them, to provide some value.

So I think it's worth-while to know that they're there and know that they're available.

But just based on the fact that they only include Admin Template Settings makes their value really limited.

So let's just kind of dig in and see what this looks like.

I'll just run through a quick scenario so you can see how to create a starter GPO and how to create a GPO from that.

---

## Starter GPOs

So, you may not have noticed, but if you go down to the bottom of a tree within a domain in GPMC, you have this Starter GPOs folder.

And the very first time that you click on it, you'll note this button here that says that you can Create the Starter GPO's Folder.

And this actually gets created in SYSVOL as a special folder in the SYSVOL area that is used to store your starter GPOs.

And what you'll notice is, as soon as I click that, it's created a bunch of Templates for Windows XP, Windows Vista, Group Policy Remote Update Firewall Ports, and Reporting Firewall Ports.

And you'll see, if you go into Settings, you can do a Settings Report just like any other basic Admin Template or any GPO that has settings in it.

And it's got a bunch of settings in it.

And what, you know, you'll notice that I mentioned previously that it only shows or only supports Admin Templates, and you're probably asking, well, why are there firewall settings here? Well what's happening is, the Windows Firewall with Advanced Security policy area actually uses the same registry keys as the old Admin Templates Network Connections Windows Firewall.

So when it sees Admin Templates for Windows Firewall defined, it translates those into Windows firewall settings.

So that's why you're seeing these here even though, frankly, these are all just Admin Template Settings.

And I'll show you this in a little bit more detail.

If I go ahead and right-click starter GPOs and say New starter GPO.

I'm going to say, Test Starter.

And I'm going to just say, This starter is for corporate standard Admin Templates.

And what I'll do is go ahead and define a couple of Admin Template settings.

So if I right-click on this and open up Edit, it brings up GP Editor but it's sort of a stripped down version.

I only see Administrative Template options for, under User and Computer Configuration.

So I'm going to go ahead and go into Windows Components.

Under Computer Configuration > Windows Update, and I'm going to use this to Configure Automatic Updates.

So I'm going to say, this is my corporate standard for Windows Update, and, you know, I say Every day at, or Every Friday at 3 a.m.

Schedule the install, and then I can use this going forward to create a new GPO.

So if I come in under Group Policy Objects and say New GPO, you'll notice that I have a Source Starter GPO that I can use.

And I'm going to pick my Test Starter and I'm going to say, Marketing Windows Update Policy.

And create from my starter.

Now if I go to marketing Windows Update Policy and go in under Settings, what you'll notice is it's picked up the settings from the starter GPO and translated them into this new GPO that I just created.

So, essentially, I've created this sort of Windows Update Standard using starter GPOs and now I can propagate that forward into any variations on that that I want to distribute.

So that's kind of a simple example of how starter GPOs can work, and hopefully you'll see some value in that.

Like I said, it's not a super commonly used feature, but it can be useful in certain scenarios.

---

## Summary

So let's go ahead up, wrap up this module.

So we talked about Group Policy Backups, and their primary goal is to restore, or roll back a GPO to a prior state.

So you create a GPO backup and then you use the restore function to back it up to wherever the backup, whatever settings were in the backup at the time it was made.

So, it can be a great undo feature for group policy.

Now you can backup one GPO at a time or you can backup all GPOs using GPMC or PowerShell.

And we talked about the GPMC method here.

By right-clicking the Group Policy Objects container, there's an option to back up all GPOs.

We also talked about Group Policy delegation, which is again available at a number of different levels, everything from GPO creation and editing, to linking, to the ability to who can edit GPOs, who can create WMI filters, who can create starter GPOs.

So lots of flexibility in the security model of GPMC and Group Policy management.

And I showed an example of how you can delegate a, in this case a OU admin like the sales admin, the ability to create a GPO and then link it to a particular OU.

Now the GP Import function we talked about is useful for two main areas, which are restoring deleted GPOs.

So you've accidentally deleted a GPO and you need to get it back, it, you can, again, import from a backup into a new GPO, all those settings that were in that deleted GPO.

And keep in mind, again, as I mentioned, you don't get any of the links that were in place on that deleted GPO.

So all the links are lost.

You'll have to re-link that GPO manually.

But you do get all the settings which is probably the most important thing.

And then, the other useful feature of Import is this notion of transferring or moving GPOs between different domains and forests.

And the ability to essentially go from like a test scenario to a production scenario with the same GPO that you've been testing with.

And in order to facilitate that, the migration table feature lets you create a mapping of security principals and or UNC pads, that allow that cross domain migration to complete without errors, or complete with the principals intact, assuming that the principals exist on both sides.

Or even if they don't, you can drop them out of the migration table, and at least they're handled that way.

Now keep in mind, as I mentioned, that the migration tables do not support any mapping of GP preferences settings, so you're sort of out of luck in that particular area.

And then finally, starter GPOs, these kind of weird feature that lets you create these reusable templates or gold images of Admin Template settings.

And, you know, it, they have some use and I showed how you can use one to create sort of a template of Windows update settings.

But I say that, for the most part, I think they have very limited use, given that they only support Admin Template settings.

---

