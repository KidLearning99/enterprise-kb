# Introduction to Group Policy

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Introduction to Group Policy |
| Type | Automated Transcript Synthesis |

## Introduction to Group Policy

So, welcome to Pluralsight.

I'm Darren Mar-Elia and you're watching module number one, Introduction to Group Policy as part of the Group Policy Fundamentals course.

Let's dive in.

So, a little bit about me.

Since I'll be your instructor for this course, I'd like to tell you a little bit about what I've been up to.

I've been doing IT and software for about 30 years.

Been a contributing editor to Windows IT Pro magazine since 1997, which is a long time.

I've written a lot of articles for the magazine, and I think actually my first article in 1996, in late 1996, was Macintosh services for Windows NT4.

So that kind of dates me a little bit.

But I've definitely been working in the Microsoft space for a long time.

I've been a Microsoft Group Policy MVP for the last 11 years.

And I've contributed to or written books related to Windows and technology and systems management.

So I've spent quite a bit of time writing words to help folks learn about Group Policy.

And this is the first chance I've gotten to kind of put it in video and give you a sort of hands on benefit of the many years, I think it's been well, 14 plus years since Group Policy came out.

So that you can sort of benefit from all that pain and tribulation around learning Group Policy and using Group Policy in a variety of environments.

When I'm not doing Group Policy, I have a software company that, not surprisingly, does Group Policy solutions.

And I write some of that code in C#, so I do enjoy coding.

And when I'm not doing any of that techy stuff, I try to be on my bike or running or doing something outside.

So, I'm definitely, I find it useful to work out the cobwebs of sitting at a computer all day.

So let's dive right in and talk a little bit about the history of Group Policy.

I think it's always useful to understand where technology has come from in order to be able to appreciate and be able to leverage it.

Group Policy was preceded by this thing called system policy in WIndows 98 and WIndows NT 3.5 and 4.0.

And system policy was really just a file that you could edit that delivered a bunch of registry settings to computers and users through logon scripts.

It was really straightforward and really basic.

But at the time, it was pretty revolutionary because you could lock down your Windows systems pretty easily using this technology.

And it was very, it was extensible.

You could create templates to add features to it.

And it was really quite the configuration management boon at the time.

But then when Windows 2000 shipped in 2000 with Active Directory, Group Policy was introduced and it really kind of took what system policy had done to the next level in terms of both the things that you could do from a configuration perspective, as well as the ways that you could target it.

Interestingly, Group Policy's name, which I've never really understood, has nothing specifically to do with groups, the way we think about groups.

It can target groups of users and computers.

But it's not really oriented around Active Directory groups.

It's just a policy-driven configuration management system.

So, the name, don't let the name throw you.

If you've heard the word or the acronym GPO, it just means Group Policy object.

So, what does it do? It provides centralized configuration control over Windows desktops and servers, and that's really its reason for living.

It's giving you the ability to lock down, secure, deploy software, map user drives and printers to desktops, to Windows servers within a Windows environment.

And in fact, there's some third party vendors out there that have gone so far as extended Group Policy to work on non-Windows systems like Linux systems and Macintosh systems, so you can use Group Policy to configure those systems as well.

So, lots of capability came out of Group Policy being born.

I think it's also useful to look at Group Policy in the context of other configuration management tools that Microsoft provides.

So, we've got System Center Configuration Manager, which was long time ago called SMS.

And SCCM is kind of their enterprise configuration management utility.

Interestingly, it's always been kind of geared towards software distribution and inventory.

That's what people know it for.

And it really hasn't done what I would consider to be true configuration management from a settings management perspective until more recently when they introduced features into the desired configuration management module.

So, you know, you could always deploy packages, setups of applications using SCCM, but it was really never what Group Policy was all about, which is settings delivery.

Now, Windows InTune is a fairly recent phenomenon.

It's a cloud-based systems management or configuration management service that you can subscribe to from Microsoft that does some kind of a smattering of the things that Group Policy does and the things that SCCM does.

So, it gives you a little bit more control over, you know, if you have a small office or maybe you're a doctor's office or something like that, and you don't want to pay to deploy System Center Configuration Manager, you can subscribe to Windows InTune and manage your desktops and even servers from a web console.

And it provides some pretty basic capabilities although they're introducing more every day.

And then finally, PowerShell Desired State Configuration is a new feature that shipped in PowerShell version 4.

And this is kind of a, it's really optimized for server application deployment and configurations for this dev ops sort of model for configuring servers as they come up with all the application bits and configuration files that are needed.

So it's yet another option for configuration.

And it's free and in-the-box, just like Group Policy is.

But Group Policy is definitely optimized for configuration management of the desktop.

That's where people use it the most.

I think I would say it's probably 60/40 in terms of people using it for mostly the desktop versus server.

And I think it does really provide the most complete configuration capability of all the technologies above.

You know, each of the products like SCCM and InTune are really good at specific things.

Group Policy is pretty good at a lot of things, which is kind of where it provides the value.

So, let's shift gears and let's do a quick demo on a real live system so you can see a little bit of what a Group Policy object is all about.

---

## Viewing a GPO

Okay.

I'm here on a Server 2012 R2 box.

And what I'm going to do is bring up what's called the local GPO.

The local GPO is a GPO that exists on every Windows desktop and server.

It's not an Active Directory based GPO, but it looks like an Active Directory based GPO.

So, I just typed gp edit.msc and it came up.

And you'll see here it says Local Computer Policy.

And you'll notice that there's a Computer Configuration side and a User Configuration side.

Again, this looks the same on a domain, Active Directory domain based GPO.

And these are some kind of fundamental things to know about a Group Policy object.

There's two sides to it, and if you'll recall from earlier, Group Policy objects apply to computers and users.

Everything under Computer Configuration will be processed by the computer.

And everything under User Configuration will be processed by the user.

So if you keep that kind of fundamental point in mind it'll greatly simplify the kind of targeting and troubleshooting group policy.

Those two sides to the GPO are really independent of each other and you may have GPOs that only have user configuration stuff.

Or computer configuration stuff, because they're only applying to users or computers.

Or you may have GPOs that have both, but the point here is that.

They are separate and they apply separately to targets.

Okay, so let's just dig in a little bit on a very popular area, which is admin templates.

And this kind of the, if you remember.

Remember when I talked about the old system policy world.

This is the successor to system policy.

This is registry based tweaks that you can do on your Windows systems.

And I'll just go ahead and click this one here, download missing com components.

And you'll see here that the setting has a number of different options.

All admin template settings have these three radio buttons, Not Configured, Enabled or Disabled.

In addition, you can put comments in admin template settings to document why you're using them or what their purpose is.

And there's help or explain text that shows or describes.

What each of the states of this policy will do.

So typically when a policy is set to not configure that's the default and it doesn't do anything.

It doesn't deliver any settings to the user computer.

When it's set to enable.

Able that's going to do something specific to that setting.

And when it's set to disable that it will typically do the opposite of that.

And the other thing to know about admin template settings is that by and large when you set one of these things the user.

Cannot undo them.

The UI for the, whatever thing is that you're configuring is going to be grayed or not even exposed.

So admit template setting, settings have that tendency to provide that kind of experience.

So that's kind of an introduction of the Group Policy Object, we're obviously going to spent a lot more time digging in.

You've only seen the Local GPO, but next we're going to talk about group policy and active directory and what active directory kind of adds to the equation.

---

## Group Policy and Active Directory

Okay now that we kind of get a sense of what Group Policy can do for you and what it's capabilities are.

Let's talk about how Group Policy and Active Directory work better together.

So, you have the local GPO, why do we need AD? Well, the local GPO is very specific to its device.

In other words it resides or is stored on that local computer.

It only applies to that computer and any users that log on to that computer.

And it provides some flexibility in terms of targeting users, so I'll talk a little bit more about this in a bit.

But you, you do have the ability to, at least on the local computer, say, a specific class of users may get this policy, but not this, or a particular user may get a particular policy, but.

Beyond that there's really not much flexibility within the local GPO.

In addition, what we'll also see is the local GPO doesn't support as many policy areas as domain or active directory based GPOs.

So enter active directory.

I like to say that AD supercharges Group Policy.

It really makes it fulfill its promise of a enterprise class configuration management system.

So it provides a way to target GPOs.

To users and computers either in the whole active directory domain within specific organizational units or within active directory sites.

It provides a replication of Group Policy objects.

So you're probably familiar with the fact that AD replicates directory data to all DCs within a domain.

Well, GPOs get replicated along with that directory data and therefore are locally available to any users or computers that need to process them.

So that those users of computers don't have to grab a GPO from across across the world in the case of a large global enterprise.

They can get their GPO information locally.

So it makes ad or it makes gpos scalable by leveraging that replication model.

And it provides a management in delegation framework for group policies.

And we'll get into this in a little bit more detail but essentially it provides a way of making group policy kind of an enterprise class configuration management system.

So let's look a little bit more about how this targeting works.

So you've got GPOs, and in this example, I've got three GPOs.

And what I'm doing is linking them at the domain level to apply to all users of computers.

At the sales OU level to apply to user computer and sales.

And at the computers OU level underneath the engineering OU.

So what this is essentially doing is giving me very fine grain targeting of the populations of users or computers that I want to process my GPOs.

Now, implied in this is that you could have potentially multiple GPOs linked within a hierarchy processed by a given user or computer and that's absolutely the case with group policy.

So group policies can be linked to an AD site, as I mentioned, which is really just a collection of sub nets, which means that you can have locations specific GPO settings.

For example, only map printer x when you're on this particular IP subnet.

You can link to the Active Directory domain itself which means that that GPO will be processed by all users and computers in that domain.

Or you can link to individual organizational units as I showed in my previous diagram.

And GPOs are inherited so a given user computer deep down in the AD hierarchy may end up inheriting tens, possibly even hundreds of GPOs in really large environments.

I've seen this happen before in very large environments.

There's nothing inherently wrong with it, except the complexity of managing that can become pretty overwhelming.

But the point is that a given user or computer can absolutely process multiple GPOs.

Each one of those GPOs could be telling it to do something different.

And there is a pecking order or a processing order to GPO processing at the user or computer.

It starts with the local GPO that's defined on that computer.

Then is processes site-based GPOs, domain-based GPOs, or domain-linked and site-linked GPOs, and then finally, OU-linked GPOs.

And we'll talk a little bit in much more depth later on about how GP processing works.

But this LSD OU pecking order is really what you want to keep in mind when you're designing your GP Group Policy deployments.

So let's kind of drill into this on a demo and see how this really works in practice.

---

## Group Policy in AD

Okay, so let's dig in and see how this whole active directory Group policy integration works.

What I'm going to do is bring up the Group Policy Management console in my Windows server 2012 R2 box.

And the GPMC, Group policy Management Console, is sort of your main management tool for managing active directory based group policy objects.

You'll see here that I've got my AD4 specified.

The domains that exist in the forest, and then under that my current domain.

And, what I see is a number of nodes underneath that, this is the Domain Controllers OU, a Sales OU, a Servers OU.

And then a special container called Group Policy Objects that literally just lists all of the GPOs that are defined within this environment, within this domain.

And these are some that I've created and some that come in the box.

The Default Domain Controllers Policy and the Default Domain Policy come in the box in AD when you install your domain.

And then some other containers that I'll get into later on in this course like WMI filters, starter GPO.

And this is where you'll see AD sites defined and any GPOs that might be linked to them.

So that whole process of linking is really straightforward.

I can just click up here on this Sales OU, right-click it and say, either Create a new GPO and Link it here, or Link an Existing GPO.

If I select the Link in Existing GPO, I can grab one of my GPOs, I call it firewalltest, and if I expand that out, you'll see now that GPO firewalltest is linked here at this OU.

And, the information about that GPO is listed here whether the link is inforced, the link is enabled, the GPO status is enabled, if there are WMI filters associated with that GPO, et cetera.

I'll go into more detail about what these mean, but what I really wanted to do is get across this notion of, what it is to link a GPO.

Now, if I go in and look at this GPO that in particular that I linked in the GPMC, and I look at the settings on that GPO, you'll see here that once that settings report comes back, I have a number of per computer settings associated with this GPO.

That means that any computer accounts is the sales OU would process those computer settings from the firewalltest GPO.

So, really straightforward in terms of how this whole notion of linking works.

I can link a GPO to the domain, in fact you'll see here that by default, Windows creates and links this default domain policy up at the domain level.

And this is, this exists on any plain old vanilla AD environment that you might come across.

And chances are it probably gets left there by most organizations and not modified much at all.

But the point is that this Default Domain Policy with computer configuration settings, in the case, some security settings, actually apply to all computers in the domain.

So, the other thing I'll mention down here is under sites.

This whole thing works the same way.

If I right-click on an existing site, I can link a GP, link a GPO that I have here.

So, if I have this test GPO, I can link it to the site.

And then any computers or users that exist within the AD subnets defined by this default first site name site would process this GPO.

So, really straightforward in terms of the linking, we'll spend some time talking about targeting GPOs and strategies for targeting GPOs later in the course.

But I wanted to get across a the GPMC as the main tool for managing both group policy objects themselves as well as the linking of those objects.

And b this just whole notion of how linking works.

---

## Inside the GPO

Okay now I want to shift gears and talk a little bit about what a GPO is.

In other words, what a group policy object is composed of.

There's really two parts to a GPO.

The first part is called the Group Policy Container, or GPC.

And that part of the object actually resides in Active Directory, so it's stored as an object with attributes just like any other Active Directory object, and has properties on it, just like any other Active Directory object.

Except that it's a special kind of object called a Group Policy Container.

And in fact if you dig into the innards of that object with an Active Directory, the class of object that it is, is a Group Policy Container class.

The other part of the GPO, there's two parts as I mentioned, this other half of the GPO is a set of files and folders within SYSVOL.

And on each DC, of course, because it's replicated to each DC, that folder for each GPO exist in SYSVOL.

It's actually called the Group Policy Template, or GPT.

So, oftentimes you'll hear that group policy has kind of this split personality, where there's two parts of it that replicate independent of each other.

One in AD and one in SYSVOL that are using essentially different mechanisms on different schedules to replicate with each other, to replicate across each DC.

So, you can get into situations where the GPC portion of a GPO has replicated to a domain controller, but the GPT or SYSVOL portion of it has not.

And I'll talk more later on in the course about what that actually means from a GP processing perspective.

But, the point is, that this kind of bifurcated existence that a Group Policy Object has can cause these kind of synchronization issues between the different parts of it.

So on the screenshot on the left you see active directory users and computers and it's not super obvious from this screenshot.

I'll give a demo of this in a second, but the under the system policies container within any given active directory domain, you'll see a bunch of subcontainers, with the names that have a big long, global unique identifier or GUID associated with them.

And that's actually the unique identifier for the GPO, its GUID is stored or is referred to by name on each of these containers.

So each of these Group Policy Containers of GPC objects represents a single GPO in the domain.

And if I shift gears and look at the file system and specifically under SYSVOL, there's a folder called Policies, and under that Policies folder, there's a a set of subfolders with, not surprisingly, a set of GUID named folders that those GUIDs correspond to the GUIDs that you see in active directory as the GPC.

So these GPT folders, group policy template folders, are the other side of that GPC object in active directory.

So, what's inside of this stuff, these two pieces? The GPC mostly contains information about the GPO.

It's things like the GPOs, well, of course, it's GUID, the version number of the GPC.

The extensions, the policy areas that are implemented in the GPO the security descriptor or the permission set on the GPO and I'll dig into those in a little bit more depth later on.

But essentially, all of these properties, the GPO's status, if it's enabled or disabled, or one part of it is enabled or disabled, all of that stuff is stored as attributes on that GPC object in active directory.

On the other hand the GPT, or the template the set of files and folder in SYSVOL is where most policy areas store their actual settings.

So when you got into GPO editor and you make a change to let's say an admin template setting, a file is created in SYSVOL in the GPT for that GPO that stores that admin template setting, the actual setting.

And that file is the thing that is read by a client processing group policy to determine what do, what registry change should I make.

So that.

In most cases, most policy areas, they store their GPT, or their settings in the GPT.

There are a subset of policy areas that also, or preferentially store their settings in the GPC, or in AD.

One example is Software Installation Policy, which actually stores both files in the GPT and SYSFALL, as well as creates a bunch of objects in active directory to represent a given software installation package that's been deployed in that GPO.

And, there are even other policy areas that only store their settings in the GPC.

So, when you edit a GPO, what you're doing under the covers, what GP Editor is doing really is updating both the GPC and the GPT with the updated information of what you've just done to that GPO.

So, in the case of the GPC, it's going to do things like increment the version number.

It's going to maybe add to the list of policy areas that you've been implementing in that GPO.

It's going to change the last modified date, and any other changes that you make to the kind of metadata, if you want to call it that, of the GPO.

It'll also write any changes to settings that you make to, in most cases, the GPT and SYSFALL.

And, that's all happening under the covers.

GP Editor is coded to basically do all that stuff for you, so you don't have to worry about the complexities of these two sides of the GPO.

And, then once that change is made, it starts to replicate to every DC in the domain.

Now, by default, when you open up GP Editor, it's, and you're opened on a domain based GPO, it's focused on, or it's using the PDC Emulator DC.

Now, you can change that in GPMC, and you can change it in GP Editor.

But, that is the default that you point to when you're making a change.

So, when you make a GPO change, it starts, or originates at the PDC Emulator DC, and then replicates out from there.

---

## Viewing GPO Structure

Okay, so let's go ahead and see what this GPC and GP, GPT stuff is all about.

So, I'm back on my server 2012 R.2 box, and I'm going to go ahead and bring up active directory users and computers.

And, I mentioned in the slide that there's a container called System, and underneath it is a container called Policies, where the GPC, or group policy container objects reside.

Now, you're not seeing it in this screenshot, because when I first start Active Directory Users and Computers, it's not set to advance features.

If I go ahead and select Advance Features, it shows me all of the, let's call them System Level Containers that aren't normally exposed in the UI.

And, if I expand System, and go down here to Policies, you'll then see all of the group policy objects that I have defined in this domain.

Or, more specifically, their group policy container object.

And, if I right-click on this and choose Properties, and go in under the Attribute Editor, and filter that just to show only the attributes that have values, you can see the kind of information that's stored in this, in the GPC.

Things like the friendly name of the GPO.

Of course, the GUID of the GPO.

The path to the SYSFALL part of this group policy object, or the GPT path.

The policy areas, or extensions that are implemented on the computer, and some other metadata.

The version number.

And, when the GPO was last changed, and when it was created.

So, this is all the metadata associated with this GPO that's going to show up in GPMC when I'm looking at the properties on this GPO.

So,that's the GPC portion.

Now, let's go ahead and shift gears, and look at the SYSFALL portion.

I've just got Explorer up here, and I've navigated to the domain name that I'm in, the SYSFALL share.

The domain name again, is a sort of junction point under the SYSFALL share.

And then within that, there's the policies folder.

And, you'll see, corresponding width named folders within the GPT that show the stuff that's associated with these particular GPOs.

So, if I click on one of them, you'll see some information in here, there's a machine side, and a user side.

This corresponds to settings that are defined under computer configuration, versus those defined under user configuration.

And, if I go ahead and bring up Machine, you'll see that I have this file called Registry.Paul.

I happen to know that that's the settings storage file for admin template settings, and a few other policy areas.

This is where registry settings that you define in admin templates are actually stored.

And,if I drill into this next one, there's a number of containers, or folders in here.

And, you'll see this inf file, if I open it in Notepad.

What this is showing me is a bunch of security related settings that have been defined in this GPO, including password age, password complexity, Kerberos policy, and some miscellaneous registry values.

So, this is basically a settings file for security settings that have been defined in this GPO.

Now, you'll notice that I've shown you two different policy areas that have two different file settings formats, one, Registry.pol, the other, GptTmpl.inf.

Well, what that tells you, if you haven't already surmised it, is that each policy area makes it's own decisions about how it's going to store settings data.

So, you can't, with any consistency, know exactly what a particular policy area, its setting storage is going to look like unless you've all ready researched it, or dug into it.

And,this is, of course, is a challenge with group policy, and the way it's grown up.

The fact that different product groups have worked on different pieces, or different policy areas, and came up with their own settings storage.

So, that's just kind of a quick overview of the GPC and the GPT.

And hopefully, that gives you an idea what these things really are under the covers.

---

## The Local GPO

For the final part of this module, what I want to do is talk about the role of the Local GPO.

Since most of this course is going to focus on domain-based group policy objects, I want to spend time now to talk about the Local GPO, and what its capabilities are, and the roles that it can play in group policy management.

So, as I mentioned earlier, the local GPO exists on every Windows device, client, or server.

Of course, home editions, certain editions of Windows, don't let you see it or edit it, but if you're working with a Professional or Enterprise version of Windows, the group, the local GPO is accessible and it's available for you to edit, and view, and do things like that.

Manage it.

It exist strictly within the local file system under this folder c:\windows\system32\grouppolicy.

And, it really is tied to a particular machine.

In addition, in Vista, and beyond, and Windows 7, and Windows 8, they added some additional capabilities around the local GPO.

While the computer side of the local GPO applies to that specific computer, you have some options on the user side of the GPO.

First of all, you have the default user side of the local GPO.

And, then you have the ability to create additional local GPOs for other user specific stuff for specific local user accounts.

So, if you've got a user account defined on the local system, you can create a GPO.

A local GPO user settings basically provide user settings for that specific user account.

You can also create two other user-specific local GPOs, one for anyone that logs into the systems who is a member of the administrators group of that system and then anyone who logs into the system who is not a member of the administrator group or a non administrator.

So you have this additional special use cases in the local GPO that you can accommodate in most modern versions of Windows.

You know, and I mentioned Windows 7 and Windows 8, but this works just as well on server SKUs as well.

So you can use the local GPO on servers, as well.

And, you know, those user specific GPOs I just mentioned all live in the same sub-directory structure or basically in the same sub-directory structure as the default local GPO, c:\windowsy\stem32\grouppolicy.

There's actually an extension on that group policy folder name for each of those multiple local GPOs.

And, you know, the question comes up, why do people use local GPOs? Well, if you have systems that aren't domain-joined, so they can't benefit from AD GPOs or you want to populate group policy settings in a base image before that image is deployed and so that you have GPOs in the image, day one or at the moment that the system goes live, then that's the role that it can play.

The other thing to know about the local GPO is it really only supports a subset of the policies found in an AD-based GPO.

There's a ton of stuff you can do in AD-based GPOs.

There's a very small subset of things you can do in local GPO, Admin Templates is one.

Some of the security settings not all, but some.

Folder redirection, I'm sorry.

Not folder redirection, but IE maintenance, which is actually a deprecated policy area as of when Internet Explorer 10 shipped.

Scripts start up and logon scripts and start, shut down and log off scripts can be used in the local GPO and that's just about it.

There's a couple others that are in there.

But essentially, there's no support for GP Preferences, which I'll talk more about later on.

But not a lot of capability in a local GPO other than the basics of, you know, those security things and Admin Templates.

And again, you know, putting these base settings in your gold image or managing settings in non-domain-join-environments is really where the local GPOs is primarily used.

---

## Working With the Local GPO

So just to finish up this module, what I want to do is spend a little time with the local GPO.

In this course, we won't be spending a ton of time worrying about the local GPO.

Because essentially, its role is limited in terms of what it can do as compared to AD-based GPOs.

So what I want to do is kind of circle back on the things that I talked about in my slides.

As I mentioned, the local GPO is stored on the local file system in c:\windows\system32 in a folder called GroupPolicy.

And you'll see here, there's a machine in the user folder in this special folder called gpt.ini, which just has some configuration information about the local GPO.

These folders in this file doesn't, don't actually exist until I first open up the group policy editor on a machine.

So when I go in and type gpedit.msc, that brings up the local GPO.

It's kind of a pre-saved MMC snap-in that Microsoft provides.

And you'll see here, Local Computer Policy, computer side, user side.

This is the local GPO for the default computer and default user.

In other words, for the computer and anyone logging into this system they'll get these settings that are defined in this local GPO.

And those are stored here in this c:\window\system32\grouppolicy folder.

Now on my slides, I mentioned that there's this ability to have multiple per user local GPOs that was added in Windows Vista.

And what I'm going to do here is show you how you can bring those up and edit them.

So, I brought up a blank MMC Console and I'm going to go ahead and say, Add or Remove Snap-In.

Bring up the Group Policy Object Snap-in and browse.

And what you'll see here are two tabs, the Computers and Users.

So I can look at the Local Computer Policy on this computer or another computer.

And if I go to the Users tab, you'll see my options that I mentioned in my slides.

So, I have Administrator and theresa.

These are two locally defined user accounts on this system that I can create specific per user policy for, then you'll the Administrators group and Non-Administrators.

These are the two other groups that I mentioned or two other per user delineations that I mentioned.

And you can have per user settings for those folks who are members of the Administrators group or not admin, members of the Administrators group.

You'll see here that I have the Non-Administrators GPO already created on here.

Let's go ahead and load that up and what you'll see is essentially, you only get the compu, or the user side of this local GPO.

So you're actually only getting the per user settings.

And if I go in here under System, I've defined this download missing COM components setting to be enabled.

Now where that's stored on the local file system is slightly different.

If I go up here to System32 and look at the folder below GroupPolicy, you'll see one called GroupPolicyUsers and under that, you'll see a CID, a well-known CID, that defines Non-Administrators.

Essentially, people that are not in the Administrators group.

And in there, you'll see a very similar file structure to what we saw for the local GPO with the exception that there's the only the User folder and in it are the settings files that are specific to that particular policy.

So in this case, I define that download missing COM component setting and it's stored in this registry.pol file.

So that's how the local GPO is stored.

That's how you can get access to the multiple per user local GPOs, if you wanted to find those for Administrators and Non-Administrators.

And, you know, there's some scenarios where that might come in handy.

If you're not using domain-based GPOs or if you haven't joined the domain yet.

And you want to have some specific lockdown for Aon-Administrators that, you know, where maybe they can't do much, but Administrators can.

Then that's an easy way of doing that within this multiple per user local GPO.

---

## Summary

Okay.

Let's summarize this module.

So Group Policy is the configuration management technology for Windows Servers and Desktops, it's in the box.

It is one of several that Microsoft provides, including Windows Intune, System Center Configuration Manager and the new PowerShell-based Desired State Configuration technology.

And, you know, despite its name Group Policy, it really operates on users and computers.

Groups do play a role and we'll talk about that in a subsequent module, but it's really operating on users and computers.

AD and AD-based GPOs introduce a lot of advanced feature, including targeting based on sites, domains and OUs, additional policy areas that are not supported in the local GPO.

And that local GPO is really there for kind of basic configuration control over individual user's computers.

It's good for, non-domain-join machines or putting in your base image to get some initial GPO settings, but doesn't really provide much beyond that basic control.

So, in the next module, we're going to dig in and really look at all of the different capabilities of AD-based GPOs, including policies and preferences and talk about where each plays a role in configuring your Windows Servers and Desktops.

---

