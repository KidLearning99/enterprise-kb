# Policy Scenarios - Desktop and Server Lockdown

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Desktop and Server Lockdown |
| Type | Automated Transcript Synthesis |

## Understanding Administrative Templates Policy

In this module we're going to begin a series of discussions that will take place over the next few modules that talk about specific policy scenarios and how we can implement those using group policy.

In this module we're going to talk about Desktop and Server lockdown.

So let's kind of dig into it.

Group policy's main job is really to do lockdown and that's really what it does best.

It started in that area.

Remember when I talked in module one about, the, it, the predecessor to group policy which was system policy.

That was all about lockdown.

It was all about, essentially controlling what the user could do on their system.

And that really is my definition for lockdown.

It's preventing a user or an application, or anything really on a system from doing something to that system.

And it could be everything as, from the, you know, simple things like disabling the registry editor to more complicated things like setting trusted site lists on Internet Explorer, and that's really what lockdown is all about.

It is primarily implemented in two policy areas within group policy, namely administrative templates and security, and those are pretty broad areas, especially security, and I will also say that there are other policy areas that can implement lockdown and I'm going to actually show you an example of one of those in this module.

But these are the primary areas where we see the things that control the Windows operating system and prevent the user from doing stuff.

So, let's talk a little bit about administrative templates.

We've introduced this in previous modules and shown examples of administrative template settings.

These are found under ComputerConfiguration\Policies&UserConfiguration\Policies.

So, they apply to both the computer and the user.

And they really are all about pushing registry values to the computer or user.

If it's a computer policy under admin templates, then it's going into the HKEY_LOCAL_MACHINE portion of the registry.

If it's the, if it's user policy under admin templates, then it's going into the HKEY_CURRENT_USER registry hive.

And that's of course the user's profile.

So there's no user policy being delivered if there's no user logged on to a system.

I mean, that's simply, just the user side of group policy is strictly dependent on having a user available.

And it does presume that the thing that's being locked down knows how to read those registry settings.

So what this essentially means is, you know, Microsoft ships a bunch of templates that implement lots of different policy lockdown, everything from IE to Windows Media Player.

The list is long and there are literally thousands of these settings under admin templates.

The key to them is that each of those components that Microsoft delivers has been coded to look for these registry values and to change their behavior based on their value.

So if you enable a policy to disable the registry editor, Microsoft has coded the registry editor to look for that registry value.

And if it finds it and sets it, and sees that it's enabled, it will essentially cancel the launch of the registry editor, and that is really, you know, a function of the fact that these applications are aware of admin template settings, and that's, that's really a key.

You can't really just create an admin template setting out of the blue and hope that the application is going to change its behavior.

The application has to be aware of that setting and has to be coded ahead of time to pay attention to that setting.

The other thing that we talk about when we talk about admin templates is that these registry settings don't tattoo the registry when they're applied and when the policy is removed.

So what this means and I'm going to explain this in more detail in a second, is that if you have a GPO and you define some admin templates settings on it and you apply those to a user, and the user logs on and gets those registry settings in their HKEY_CURRENT_USER registry hive.

And then you delete or unlink that GPO or, for some reason, that GPO no longer applies to that user, well then you're going to get that registry value, those values are going to be removed the next time the user processes policy.

And that gets done automatically, you don't have to do anything to get that removal to happen.

And that was never the case in the old system policy day and it was a big improvement when Microsoft shipped admin templates.

Now, in order for this non-tattooing behavior to happen, the admin templates have to write values to one of these four policy keys, two of them on the computer side under HKEY_LOCAL_MACHINE, and two on the user side.

And if you look into the template files that Microsoft ships in the box, you'll notice that 99, if not 100% of the registry keys that get written to exist under these four policy keys.

And the applications, of course, have been coded to look in one of these four policy keys for these registry values.

So these keys are kind of key to the non-tattooing nature of policy, and they're there on purpose, they're not accidental.

Microsoft has specifically coded the group policy engine to look in these keys when it's doing it's cleanup and essentially, you know, use these keys to write new values to.

So let's look at tattooing them a little bit more and how this works.

So we've got our client, we've got a GP, and the client processes an admin template setting and makes a bunch of registry tape changes on its registry.

And then a new processing cycle happens, and the client again looks for GPO changes, but before it does that, it actually removes the old values that were processed in that first cycle.

So the first thing it does is it removes those registry values that were delivered by that GPO during the first cycle.

And then it gets its list of GPOs that apply and brings down the new settings from the GPO.

So if you've gotten, you know, a change in settings, you added new settings to the previous GPO, or maybe you've even just removed all the settings from the previous GPO and set a new set of admin template settings.

Those old settings are removed before the new ones are applied.

So essentially what you see, the effect of this, is this kind of non-tattooing nature of group policy admin templates.

And it's really, the key to it is that those old values are swept away from those four policy keys I showed in the previous slide.

And that's essentially how the group policy engine is able to guarantee that no registry values are tattooed between one processing cycle and the next.

---

## How Registry Tattooing Works

Okay, so what I want to do now is illustrate some of these concepts around administrative template lockdown that we just talked about.

So, I've created a GPO link to the marketing OU called LockdownPolicy, and I'm going to go ahead and edit this GPO.

And I'm going to bring up the user side Administrative Templates.

And I'm going to do something simple.

I'm going to prevent access to the registry editing tools, in other words, registry edit or regedit.

And I'm going to also disable reg edit from running silently, and I'm going to go ahead and enable that policy.

Now what I'm going to do is come over to my client machine and go ahead and run a GP update.

Well, first I'm going to show you that, yes indeed, registry editor runs.

And then I'm going to go ahead and come to the command prompt, and issue a gpupdate on the user.

And we'll let that run to completion.

And what that's going to do is process that new registry restriction policy, registry editor restriction policy, such that now, if I type regedit, you'll see here that I get the message, Registry editing has been disabled by your administrator.

Great, so essentially now that policy has been deployed to me, and I'm now being affected by it.

I've essentially locked down from using the registry editor.

Now what I want to do is show you how if I come to this policy and essentially unlink it or disable the link so this link is no longer valid within this OU, this marketing OU.

Come back to my client and again, issue the gpupdate.

And now, try to run Registry Editor.

Sure enough, it comes up just fine.

And while I'm in Registry Editor, let's look at some of the policy areas that I talked about, those keys.

So, for example, here you'll see under each HKEY_CURRENT_USER\Software\Policies, I've got a number of policies that have been delivered to me related to, various admin template settings that have been defined.

In this case I have some Internet Settings that define Cache properties.

I've got some PowerSettings.

And I've got some App Management settings, and this particular one is the always download missing com components policy setting.

So, kind of gives you in a little bit of an idea of how this works.

So what essentially happened with the registry editor is I applied the policy, and it delivered a key to one of these HKEY_CURRENT_USER policy sub keys.

And then I unlinked the policy and did a gpupdate.

And it applied that, or removed that registry editor restriction that had previously applied.

And then essentially the registry editor was then available to me as it always was without the policy in place.

---

## Administrative Templates and ADMX Files

So let's move on from our initial discussion of administrative templates and talk about how those things get populated.

The policy options that you see in the GP Editor, where do those come from? Well, in fact, they have a long history of being provided by what are called ADMX or ADM templates.

What you see in the GP Editor under that Administrative Templates node, all those different sub-folders, are generated dynamically based on the ADMX template files that exist on your system.

And those files live in the file system on every Windows system in this c:\windows\policydefinitions folder.

They have essentially changed very little over the years.

But starting in Windows Vista and Server 2008, Microsoft moved the format of those files from simple text files where both the policy registry entry definitions and the strings that you see, the words that you see in GP Editor, were combined in one file in the ADM world.

To this new world of ADMX files where the ADMX file contains the registry values that need to be supported.

And the ADML file, the file which the .adml extension contains localized strings.

So, depending on which language version of Windows you're running, what you'll see in GP Editor will be localized policy settings.

So if you're on a French version of Windows, you'll see French text appearing under admin templates.

Or if you're in on an English version of Windows, you'll see English text.

So the ADMX format which is now also XML makes it a lot easier and more flexible when it comes to supporting multiple languages and multiple sources of information from a sort of design perspective.

But the bottom line is that ADMX files and, just like ADM files before them were the things that drive what you see in GP Editor.

So let's look at a little bit more depth into these things.

So this is just a screen shot of the C:\windows\PolicyDefinitions folder on one of my test machines.

And you'll see all these ADMX files.

There's 176 ADMX files that Microsoft ships out of the box in this particular version of Windows.

And I can add to this, depending on what I'm trying to control.

In addition, if I drill into this, you'll see a subfolder called EN-US, and that stands for English US.

And in that subfolder, all the ADML files, there's one ADML file for each ADMX file.

And it contains, those ADML files, contain the language specific strings that get populated into the GP Editor when these things are loaded.

So how does this correspond, really, to what you see? So what I've done, is I've taken a screenshot of the GP Management Editor, and drilled in under Windows Components within Admin Templates and AutoPlay Policies.

And there's a set of four options on the right hand side of the screen.

How this actually gets created and populated is driven by the contents of the AutoPlay.admx file.

So that AutoPlay.admx file gets loaded up when you load GP Editor, parsed, the XML is parsed, merged with the strings in the ADML file, and essentially turns in to what you see in the GP Editor.

This folder that says AutoPlay Policies and the settings, those four settings, and their options are all defined in those ADMX and ADML files.

Note that, and I see this, sort of confusion, a lot.

ADMX and ADML files are only used by admin template settings.

No other policy area, be it security or GP Preferences, or Internet Explorer's, maintenance, or folder redirection.

None of those other policy areas are controlled by ADMX files.

Only what you see under User Configuration Admin Templates, or Computer Configuration Admin Templates.

Now, this notion of having ADMX files driving what you see in GP Editor is completely extensible.

You can extend what you see in the GP Editor by creating your own custom ADMX and ADML files.

Now again, you can arbitrarily populate registry settings using these ADMX and ADML files to define those registry settings.

And in fact lots of third parties do this, Citrix, Firefox Mozilla, Google Chrome.

Even other Microsoft products like Microsoft Office provide their own ADMX files that let you configure those products.

And it's important to recognize that when you write your own, it has to follow the proper schema and format for ADMX.

And, in order to do that, you would typically use some kind of, you could certainly use Notepad or some thing that creates text files.

But Microsoft has a free tool called ADMX Migrator.

It's initial role in life was to let you migrate or convert ADM files in the old.

Windows XP days to ADMX files, but it also has an authoring editor that you can use to sort of jump start writing these things.

And again, it's, you can configure any registry value using ADMX.

But if it's in not in one of those four non-tattooing registry keys that get removed each time Admin Template policy is processed, then anything you configure via ADMX will tattoo the registry.

So that's just something to keep in mind, you know, that you're free to create these custom ADMX files.

But they don't necessarily fall into the non-tattooing behavior, unless the application that you're trying to configure or control will look in those four keys for their configuration settings.

So, I mentioned that ADMX and ADML files by default are stored on the local workstation under c:\windows\policydefinitions.

So, each person that runs GP Editor, will consult that directory as GP Editor comes up to build the tree of stuff that you see under Admin Templates.

Now there is the facility to do something called the ADMX Central Store and a Central Store is really just a copy of all the local ADMX and ADML files stored in the SYSVOL folder that's replicated to every domain controller in an Active Directory domain.

And specifically, you would copy the contents of your c:\windows\policydefinitions folder to this folder structure under\\<domain name>\SYSVOL\<domain name>\Policies\PolicyDefinitions.

And once you do that, everyone in the domain that's editing GPOs or viewing or reporting on GPOs using GPMC and GP Editor will refer to that.

So, it essentially overrides what you have under c:\window\policydefinitions and everyone uses the same ADMX files.

---

## Administrative Templates and ADMX Files

Let's dig into the ADMX files a little bit more and see what they're all about.

So I've got the c:\windows\policydefinitions folder up and I took the liberty of associating my ADMX extensions with Notepad, so it's easy for me to bring up an ADMX file.

And what you see here is the AutoPlay.admx that I showed in my previous slide.

And it's XML and not necessarily something that's intuitive to Author and that's why I recommended using an authoring tool of some kind.

But essentially, how it works is you have a policy and this is a policy tag.

It's got a name, it's got some strings associated with it.

And these strings are defined in the ADML file for the particular language that you're working in and then it's got some explain text.

Again, in the ADML file.

What kind of presentation it uses? Is it a checkbox or list box, that sort of thing.

What key is affected by this policy and you'll see it's one of our four policy keys under CurrentVersion\Policies.

And then it, if you go down underneath this policy, you've got some more elements.

And this is the value in the registry that's set and then it tells you if the value is set to Disabled, then the actual value of the registry is set to 1.

If it's set to NoAutorun_XP, then it's set to 2.

And these all mean something, the strings correspond to text that's defined in the ADML file.

So essentially, that's how the ADMX file works.

But we can also do is drill into this en-US folder and see all of the ADML files.

And if I go in under the AutoPlay.adml and I'm going to tell it I want to open that with Notepad as well.

You'll see how each of the strings defined in the ADMX file have corresponding text associated with them.

So the various things that you saw in that ADMX file are translated into nice, easy to read text within the ADML file.

So let's look at how we can create the Central Store that I mentioned.

So what I'm going to do, I have my ADMX and ADML files defined in this windows policy definitions folder on the local machine and I'm going to go ahead and copy all of these files.

What I'm going to do is I'm going to go ahead and bring up the explorer on this PC, I happen to be on a Windows domain controller.

So, I'm going to go ahead and drill into the SYSVOL folder that's shared out as sysvol on this windows to main controller.

And I want to drill into Windows > SYSVOL domain policies, because that's where that folder is shared from.

And I'm going to go ahead and create that new policy definitions folder.

And then I'm going to paste the contents of my local policies definitions folder into this one.

And now, I have a Central Store, that's really all there is to it.

Now, if I were to go in and reload GPMC.

Go ahead and run that again and edit this policy and expand Administrative Templates.

You'll see here, if I highlight the Administrative Templates node, it says, policy definitions are being received from the Central Store.

And that's telling you that indeed, all of my Admin Templates settings or all of my underlying Admin Templates that are building these trees of policy settings are coming from the Central Store.

---

## Deploying Administrative Templates

Okay.

Now I want to talk, since we've kind of laid the groundwork on what Administrative Templates can do.

I want to talk about how you can deploy them or strategies for deploying them to get the best out of them.

So, you know, if you're face with kind of a blank slate, you've got 1000s of these Admin Template settings in the box.

It's shipped by Microsoft via ADMX files.

Where do you start? Well, you know, there's a couple places that I typically recommend to folks.

I mean, typically you know what you want to lockdown in your organization and at least at a high level you understand, you know, that you care about things like IE.

You might want to prevent the user from doing certain things on their desktop, like running registry editor or running the command shell.

You have a high level idea about what you want.

The Microsoft provides a setting spreadsheet, you can download from the Microsoft site.

That is a kind of handy way of listing all of the available Admin Template settings that ship in the box for each version of Windows as it's released.

And there's also a neat application online at this URL gpsearch.azurewebsites.net, where you can do a keyword search on Admin Template settings.

And find, let's say, you wanted to find a setting for Media Player.

You can just type in Media Player and it will return all of the Admin Template settings that have that in their explain text or in the description.

And so, it's a great way of finding settings.

I think discover ability in those thousands of settings is a big challenge, so that provides kind of a starting point for you.

And then when it comes time to deploying those settings, what I like to do is.

I mentioned earlier around not having one setting per GPO, or at least not going to town on that idea and having hundreds of GPOs, each with one setting.

So one of the ways that I think you can get around this especially with admin templates is to group settings based on their function.

So, for example, you might have a GPO that defines all of the IE settings for the domain or for the business unit.

Or a setting that controls the, kind of the user experience.

The Start Menu, Explorer options, desktop, et cetera.

And then you might have a separate setting or separate GPO that has only Microsoft Office settings.

And that way you can more granularly target these GPOs at the right audiences, and that can be, you know, kind of a really helpful thing as you're deploying into finding these GPOs.

So think about grouping based on function or based on need, or the use of the policies rather than just sort of randomly picking a new GPO and creating a setting in it and applying it to all users or some of your users in the environment.

Now what this looks like is really illustrated in this kind of a graphic on a typical AD domain.

So, you've got your domain, you've got a people OU and then underneath the people OU, you have a marketing and a sales OU.

And at the people level, we've got a couple GPOs, the all users lockdown and all users IE and each of those go to all of the users underneath this OU.

So these are settings that apply to every user regardless of whether they're in marketing or sales.

And then what we do is more granularly and closer to the ultimate target, you might have some separate lockdown for marketing.

Maybe you don't want marketing to be able to view a certain drive letter, or you don't want them to be able to open the command prompt.

Well, this is a place you can do that.

You can link a GPO with more specific, not conflicting, but more specific lockdown specific, to, for the marketing folks.

And similarly, sales, you could have their own lockdown GPO that has settings that are specific to sales.

So what happens is if I'm a user in sales, I essentially get the sales lockdown GPO, the all user lockdown GPO, and the all users IE GPO.

And all of those apply to me at GP processing time to result in what the sales person would get.

Similarly for the marketing person.

So in this way, you sort of build a layered approach, and this approach actually works beyond just admin template settings.

It works for security, policy, it works for a lot of different policy where you're linking the settings as close to their intended target as possible, which is a guideline that I mentioned early on, and you're really, you know, trying to get very specific about a particular audience.

So, the general stuff is linked higher up so that you only have to define it once, and then the more specific stuff is linked lower down on this specific target audience.

---

## Summary

Okay, let's summarize what we've learned about admin templates in this module.

So Group Policy's main job is lockdown and this is implemented primarily in admin template policy.

Security policy certainly has a role here and I'll talk about that in the next module, but group policy's history comes from admin templates and the capabilities of being able to push registry values into a given computer or user.

So, admin templates generally does not tattoo a system, this is a development that Microsoft made great strides on when they introduced group policy.

When you apply a group, an admin template setting to a system or a user, it gets applied and pushed there.

And if you remove that GPO, the next processing cycle that happens will remove that old setting from the system and reapply the new settings.

So that tattooing doesn't happen, and that's a great thing.

You can extend what can be controlled using admin templates.

So these underlying ADMX and ADML files that are the things that generate what you see in GP editor, those can be extended.

You can write your own custom ADMX and ADML files, and I gave you a link to a free Microsoft tool called ADMX migrator that has an authoring tool in it that lets you do these things.

They are, very powerful in that you can really customize any registry entry or any registry edit that you want using an ADMX or ADML file.

And of course, if it's not in one of those four special policy keys I talked about, it will probably tattoo the system, but nonetheless, you have the flexibility to really write a custom ADMX for really any registry value you want to set.

You can deploy admin template settings in layers.

So what I had suggested it is that you can provide kind of base level lockdown for general things like Explorer and the Start Menu, and the desktop.

And then if you have other more specific, application-specific lockdowns you want to do like IE or Office, you can create separate GPOs that contain all the settings for IE, or all the settings for Office for a particular business unit.

And this is a good way of kind of approaching the problem from a layering perspective, not duplicating but providing kind of an additive use of admin templates, and taking advantage of the GP hierarchical processing, model that group policy has.

And then, finally, in terms of discoverability, you know, there are literally thousands of admin template settings out there, so how do you find them? Well, there's the Microsoft settings spreadsheet that I talked about, you can get from the Microsoft Download site, or the GP Search app, it's a online web app found on the Microsoft Azure site, developed by somebody at Microsoft, and it gives you the ability to interactively search on keywords.

And essentially, what it does is discover within the current admin templates that Microsoft provides, and it is specific to the Microsoft admin template settings.

It can give you the ability to search on settings to find a particular area that you're interested in locking down.

So, that's a great resource and, what we're going to do next is talk about how security policy lends itself to this whole notion of lockdown.

---

