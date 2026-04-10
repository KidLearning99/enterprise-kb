# Policy Scenarios - Managing Internet Explorer Configuration

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Managing Internet Explorer Configuration |
| Type | Automated Transcript Synthesis |

## The Three Policy Areas for Managing IE

All right.

In this module, we're going to explore the management of Internet Explorer configuration using group policy.

So with IE, you would think Microsoft would be the best at configuring IE through group policy.

And unfortunately, that's not the case.

There's been a very mixed history around managing IE configuration using group policy.

So before Windows 8 shipped, there were essentially three ways to manage IE settings or IE configuration.

The first way was with good old Administrative Templates policy.

So every new version of IE would ship with new sets of admin template ADMX files and ADML files that provided settings control over features within group policy or within IE.

There was also, from the very beginning, an area called IE Maintenance Policy and this was another way of configuring sets of options within IE and I'll talk a little bit more about this in a bit.

But it was not necessarily a complete overlap with Administrative Templates.

It did more than what you could do through Administrative Templates.

And then finally, when GP Preferences became part of the group policy universe.

Microsoft added the capability of configuring IE through Group Policy Preferences Internet Settings area.

So there were lots of different sometimes conflicting ways to configure your IE settings and users were often not clear on which one should be used under which circumstances.

So what Microsoft did, interestingly is removed one of the options.

In other words, IE Maintenance Policy was removed from Windows 8 and any system that had IE 10 or greater installed.

So we were left with those two ways, Admin Templates and Internet Explorer or a Group Policy Preferences Internet Settings as the two main ways to configure IE today.

So what happened to IE Maintenance Policy? Well, on systems running Windows 8 and above or any system, Windows 7 for example, with IE 10 or above installed.

The Internet Maint, IE Maintenance Policy under User Configuration went away, it just disappeared.

No more processing of IE Maintenance Policy by users.

So in other words, if you are a user that had previously been running Windows 7 with IE 9 and you upgraded to IE 10 and of the Internet Explorer Maintenance settings that you had previously been getting would not be delivered to IE 10.

It simply stopped working, no more editing of IE Maintenance Policy.

It just simple disappeared from the policy editor if you were an administrator of these settings.

Now reporting on existing settings would still work, so you can still see IE Maintenance settings within the GPMC in existing GPOs.

You just couldn't edit them and you couldn't process them if you were a user in, that was receiving these settings.

Now, it's not really all that bad in the bigger scheme of things, because IE Maintenance has had a history of being super buggy.

There were problems with IE Maintenance almost from the beginning of its introduction.

So, I wouldn't say that I'm heartbroken about IE Maintenance going away.

But it's definitely, the way it was done, I think has caught a lot of users by surprise.

And so we'll talk about sort of how to go forward in this new world where we have these two remaining areas.

And I think the biggest challenge that you're going to find is that, IE Maintenance actually did some pretty cool stuff from a configuration perspective.

And there is no one to one replacement in the remaining two policy areas for all of the stuff that IE Maintenance did, so you sort of have to resort to registry hacks and other methods.

To try to configure IE if you were using some of the more, lets say, advanced setting within IE Maintenance Policy.

So let's see how the remaining two ares fill the bill.

We've got Admin Template settings.

Now they don't cover all of the things you can do in IE from a configuration perspective, certainly not everything that IE Maintenance did.

But the, the key kind of distinguishing factor about Admin Templates settings for IE is that they prevent the user from changing settings.

So if the user, if you configure Admin Template settings for the user's browser, they will not be able to change those settings.

Now alternatively, GP Preferences what it brings to the table are more configuration options than what you have in Admin Templates, so a little bit closer match to what came in IE Maintenance Policy.

And it's sort of easy to use, I'll show you this in a subsequent part of the module.

But it kind of mimics the IE interface exactly, so it's almost like you're in IE configuring IE, but it's really just GB Preferences.

And then the other thing that it doesn't do that Admin Templates does, is enforce configuration.

So the user can actually change things that you specify in GP Preferences Internet Settings and that's a little bit different than what you might find in Admin Templates, which obviously the whole goal of that is to lock down the interface, user can't change it Okay.

---

## Introducing the Three IE Policy Areas

Let's dig in a little bit on these different areas for configuring IE.

I've got Group Policy Editor opened on my Server 2012 box.

First off, let's look at Admin Template Policy.

So, either under Computer Configuration or User Configuration.

If I drill into Admin Templates, Windows Components, Internet Explorer, there are a bunch of options available for configuring Internet Explorer settings.

So everything from pop-up allow lists to turning off features within the UI, IE to security settings.

So there's lots and lots of capabilities around the various settings within group policy or within IE.

Now as I mentioned, Admin Templates is supported both per computer and per user.

So you can either set, if you set it per computer, you're setting for all users who log into that machine.

If you're setting it per user, you're setting it for whichever user accounts in AD process the per user policy as they log onto a machine.

So a little bit different approach.

One is per computer, where you can sort of enforce for all users, and the other is on a per user basis.

Now with the other two areas of configuring IE, it's only per user.

So what I'm going to show you, since I'm on a 2012 box running IE 11, you're not actually going to see IE Maintenance Policy.

Normally, it would be under here, under User Configuration window settings.

But I will show you group policy preferences under User Configuration > Preferences > Control Panel > Internet Settings.

Now, if I right-click on this, I can get in and create a policy that's based on the browser version and then you'll see all the options available to me.

Now let's shift gear quickly and I'll just show you what this looks like on a machine that does have only IE 9 installed and you can actually see Internet Explorer Maintenance Policy.

So you'll see under User Configuration > Windows Settings > Internet Explorer Maintenance Policy and I can view the options that I can configure.

I can configure Browser Settings.

I can configure Connection and Proxy Settings, etc.

All of these are available, because this machine is still running IE 9 on it and it's only Windows 7.

So, you know lots of nuances to how this works based on which browser version you're using, which machine operating system version you're using for editing group policy.

It's important to kind of keep this in mind.

And, I'll underscore this a little bit further on in this module, but my basic recommendation is if you're using IE Maintenance today, move away from it, and if you're not using don't start using it, because eventually it will go away.

---

## Using GP Preferences Internet Settings

Now let's dig in to Group Policy Preferences: Internet Settings and see how we can use this to configure IE.

So, the first thing to note about it is, I think I showed you this in the UI, but you can configure different browser versions using these group policy preferences internet settings options if you right click on the node and say new.

You'll see all of the different browser versions that are configured or that are configurable using group policy preferences.

Again, note that this is only per user, so you can only configure this and target it at user objects in AD.

Now what you're seeing in this screenshot reflects what's available on Windows Server 2012 R2 and/or Windows 8.1.

And what's a little bit misleading is that even though the last option says Internet Explorer 10 only, it actually will support configuring Internet Explorer 10 and 11.

So, similar to the one before it, IE 8 and 9, you can configure IE 10 and 11 using this last option.

And so, you can support multiple browser versions using the options that I showed here within a particular GPO.

So you can have configuration settings for IE 5 and 6, IE 7, IE 8 and 9, and IE 10 all in the same GPO.

And group policy preference is smart enough to target only those machines that have the appropriate browser version installed for those settings.

So that's kind of a handy thing to be able to do.

Now with the group policy preferences UI once I dig into it, you'll see that it looks a lot like the tools internet options menu selections that you see in when you're in IE.

So the UI for configuring group policy preferences is almost identical to the browser that you see, the option that you see in the browser when you configure it.

Now each option that you see that's underlined for colored has and is individually settable.

So you could click that delete browser history on exit, and that will set that option within the group policy preferences.

So there's obviously ways to tell it which settings you want to configure and which you don't.

Red, the dotted red line that you see under the homepage, that indicates that the setting is disabled.

That means that group policy preferences won't configure this on the client when the user processes this policy.

Now if it's green like the delete browsing history on exit option, well that means that it will configure it that it's enabled.

And you can actually control these green or red toggles either on per setting basis or on an entire tab as you see it here in the UI.

So Microsoft has made these kind of secret hotkeys available, F5, F6, F7, and F8.

And they can be used to configure, or turn off or on, or disable or enable each of these options.

And these, these hotkeys, these F, F keys, are actually universal to all group policy preferences.

Not just the internet settings area.

This works across all the different areas.

So you can either enable all or disable all, or enable or disable specific ones or this particular page or a particular setting using these hotkeys, these F keys.

So it's really kind of a powerful capability.

The thing to keep in mind here is let's say, for example, you put a homepage entry in and left the line as a red underline.

Well, it's simply going to be ignored by the user that processes this policy because it's set to essentially disable.

So some of the common things that you can do with GP preferences.

You can set the home page.

You can set security levels for zones.

So Internet Zone, Intranet Zone, Trusted Sites Zone, or Local Zone.

You can set the security levels that the browser configures for each of those zones using GP preferences.

Now you can't do what's called site-to-zone assignments, which means you can't add particular domain names that should be placed into particular zones.

So you can't say that, for example, www.microsoft.com should always be put in the trusted site zone and therefore have a different level of security.

You can do that Admin Templates, but it's not supported with TP preferences.

You can do things like controlling the pop-up blockers behavior and the sites that get exceptions to the pop-up blockers.

So you can add a web domain that does not respect the pop-up blocker if you choose to do so.

And you can control which versions of SSL and TLS are supported in the browser, so you can configure that through GP Preferences.

And most importantly, you can set proxy configuration.

So you can set which, the address of the proxy server for the browser or any other options related to proxy, exceptions to the proxy and port specific proxy configurations if needed.

You can control IE history and cache behavior.

So how long history is kept, where it's kept, whether the cache is cleared on browser exit.

Those kind of options are all configurable in GP preferences.

---

## Using GP Preferences Internet Settings

So now I want to dig in and actually do some configuring of internet settings using the preferences feature.

So I'm going to go ahead right click the internet settings node and select new.

And I'll go ahead and select the Internet Explorer 10, which as I mentioned earlier covers both 10 and 11.

And I think what I'll do, just as an example, is set proxy configurations since this is a pretty common task that folks need to do when configuring IE in corporate environments.

So if I go into LAN settings you'll notice that I have the option of selecting automatically detect settings.

And that let's IE detect its proxy configuration automatically.

And if I go ahead and click into this address field, what this is looking for is a automatic proxy configuration script or PAC file.

So I can specify that here or down below I can specify, use a proxy server for your LAN.

And you'll notice that these are red.

So what I'm going to do is go ahead and select F6 to turn this green and give it a port and address of my proxy server.

So we'll just call it proxyprod.

And we'll say that it's using port 8080.

And I want to bypass the proxy server, so I'm going to set this to green as well and check it.

And if I go in under advanced, you'll see I have the option of selecting the HTTP protocol, or I can uncheck this box here and give it a different port number for each of the different protocols.

So for example HTTPS or secure might be 8088, FTP might be 8085, et cetera.

And I can go ahead and click OK and close that out.

And now I've set the proxy for this particular GPO.

Now let's say I wanted to set the home page.

Again note that the home page option is.

Basically read.

So if I click F6 again, I can say set the homepage to mycompany.com.

So maybe I have a Intranet site and I can go ahead and do that.

Now, there might be other pages where I don't want to set any settings at all.

For example, on the Advanced page, I may not want any of these to be set in IE, and you'll notice that by default, when I created this brand-new GPO, there are a bunch of these options that are already configured.

So, I can go ahead and click F7 to delete or to disable individual settings or F8 disables all of these setting, so that essentially the client will not, or the user will not process any of these settings.

So it will leave the user's IE default settings as they are.

And that's kind of a nice feature of those function keys is it lets you quickly kind of go through and either selectively on a single setting or across all settings within a tab, you can quickly and easily define whether you want those settings enabled or not.

---

## Using IE Administrative Templates

So now, let's talk about admin templates.

So, admin templates as I showed in a previous demo is both per computer and per user, so you can find those settings under Computer Configuration or User Configuration policies.

AdministrativeTemplates\WindowsComponents\InternetExplorer and there's a lot of options within those.

There's some sub-folders under there for all the various pieces of functionality like privacy, security, internet control panel, etc.

Lots of, detail in the admin template settings for Internet Explorer and if you're wondering where these options come from and how they get updated.

The Admin Template file that underlies the Internet Explorer options, is called INETRES.ADMX, and it's got an associated .ADML file.

And this is the file that stores all of the settings that you see in admin templates.

This file actually gets updated, if you update your browser from, let's say, IE 10 to IE 11.

This file automatically gets updated in your local ADMX store, so under C:\ Windows\Policy definitions, you'll notice after you install IE 11 that you've got a new version of this file that contains all the additional options that you can configure using Administrative Templates in IE 11.

So lots of good functionality in that ADMX file that lets you lock down and enforce the browser configuration for your users.

And again, remember that the distinguishing feature here of IE is that it enforces, I'm sorry, of Admin Templates is that it enforces the settings on the user, the user cannot undo those settings.

So it makes it very, very locked down in terms of what the user gets within IE.

And this is good for kiosk type of configurations, or public workstations where multiple users might be coming and using IE, and you don't want them fiddling with those settings.

So, typically, admin templates is a good place to do that lockdown.

Now, some of the other things you can do with IE admin templates, you can set the home page, just as you could with GP preferences, and again you're setting it and locking it so the user can't change it, versus in GP preferences the user will get that new home page, but they can go in and change it as they see fit.

site-to-zone assignments, I alluded to these a little bit earlier.

This is where you can define which sites belong to which zones from a security perspective, so you might have some corporate intranet sites that you always want to be trusted with lower security.

You might even have some Internet sites that you do business with on a regular basis.

You want those to be trusted, because you know they're good sites.

You can stick those into specific zones, using the site-to-zone assignment lists.

Zone security, just as you could with GP preferences, you can set each of the different zones to, you know, low, medium high, medium low, etc.

So, that capability is also available in admin templates.

And again, it locks the option so the user can't change it.

Pop up blocker and pop up blocker exceptions can be set through admin templates.

And then, you can use it in conjunction with GP preferences to lock down areas that have been set by those preference settings.

And what I'm going to do in my next demo is show you an example of using both admin templates and GP preferences in conjunction to sort of get the best of both worlds.

So you get the capabilities to lock down the proxy that are included in GP Preferences and not in Admin Templates.

And if you don't want the user to change that, we're going to show you how you can use Admin Templates to essentially lock out the options so the user can't mess with it.

---

## Using IE Administrative Templates

Okay.

So now what I want to do is create a scenario to illustrate some of the admin template settings for IE, and how they can work with group policy preferences internet settings.

So, I've created this GPO called, IE Lockdown policy.

We're going to go ahead and bring up the GP Editor on it.

And first, let's go into Admin Templates.

I want to show you the site to zone assignment list that I mentioned,Admin Templates was good at setting earlier.

If I go in under Internet Explorer.

This is per user now that I'm setting this under User Configuration.

I'm going in under Internet Explorer > Internet control panel > Security page.

If I come over here to the right you'll see this policy called Site-to-zone assignment list.

Now, this is very frequently used, and I'm going to go ahead and bring it up.

If you look at the help text, it explains that you can assign websites or web domains to particular zones using numeric values, so one for internet zone, two for trusted sites, three for internet zone, and four for restricted sites.

So if I enable this policy, I can go ahead and type in some domain names that I'm interested in assigning to particular zones, and so I'll assign Microsoft.com and Google.com to the trusted site zone, and now those are in there and the policy has been set in this GPO.

Now what I want to do is, by contrast, go into GP Preferences and set a couple of Internet Explorer settings for IE 10 and above.

So, I'm going to go ahead and double click this existing one.

The first thing I want to do is set the home page to Google.

So, I'm going to go ahead and set the home page to Google.com.

Then, what I'm going to do is go in and define a proxy setting.

And I've already got a proxy setting defined here.

It's a proxy PACK file.

This is a central file that can be used to set proxy settings on a browser so let's go ahead and accept that.

And so now I've got these two things set.

I've got the homepage set in GP Preferences and I've got the Proxy set.

I'm going to go ahead and say OK to this to accept those changes.

And now what I want to do is take this GPO and link it to an OU that contains a user that I have in my test environment.

So I've got the IE lockdown policy.

I'm linking it to this user's OU underneath the sales OU, and I've got a user called Joe Sales on this machine here, this Windows 7 machine.

So this is me as Joe Sales, logged into this Windows 7 machine.

Now, if I bring up.

Internet Options on the browser.

You'll see here that currently, the home page is set to microsoft.com, that I don't have any trusted sites defined and I don't have any proxy settings defined.

Okay, great.

So now, let's go ahead and run gpupdate on this machine.

And I'm going to use the force parameter, because it always helps me make sure that I'm getting the most frequent or the most recent Group Policy settings that have been delivered.

So, as soon as that finishes running, we'll say, no to logging off.

And let's go back in the browser and see what's changed.

If I go in under Internet Options, you'll see now that my home page has changed to google.com.

If I go under Security > Trusted sites > Sites.

Now you'll see Google and Microsoft have been added to the trusted site zone and I can no longer change anything.

So this is one of the side effects or one of the capabilities of Admin Templates is that when it makes changes it prevents me from making any subsequent changes to those.

So, I can't edit these trusted sites that have been added by the administrator.

And you'll see here, this message.

It says, some settings are managed by your system administrator.

Now if I come in under Connections, let's look and see if we've gotten my proxy setting.

We haven't gotten my proxy setting, at least not visibly.

But what I'll do here is close the browser.

Bring it back up, let it go to Google.

And go back in and see what I get.

And there it is.

And the reason that happened is because there are some settings that will require the application that's being configured to be restarted.

And in this case, a proxy configuration is one of those.

Now, what you notice here, also is that I can change this.

I can disable using this automatic configuration script.

And if I come back into it, it's still disabled.

So because this was delivered as a preference, it's perfectly reasonable for me to go ahead and change it back.

Now that's probably not a great behavior.

If I'm an administrator, I want to be able to enforce something like proxy.

So what I'm going to do is go into my IE Lockdown Policy again.

Go ahead and edit it.

I'm going to come down to Admin Templates > Windows Components > Internet Explorer.

Let's go ahead and expand that out a little bit and then I'm going to go ahead and sort these Settings.

And if I go down here, you'll see an option that says, disable changing connection settings.

That sounds about what I want.

So, I'm going to go ahead and enable that.

So now that's enabled on this GPO that is being processed by Sales.

So now what I want to do is come back to my Windows 7 client.

And go ahead and do my gpupdate.

And once that finishes running, I'm going to go back into IE and see how, if anything has been affected by that policy I just set.

So let's go into Internet Options.

Connections and look, the LAN Settings button that used to be available to me to change is now disabled.

So now that proxy is set by GP Preferences, but the LAN Settings button is disabled by Admin Templates.

So, I'm taking advantage of the power of both of these different settings to be able to, essentially configure proxy in one case using GP Preferences and disable the ability for the user to change it, what they would normally be able to change using Admin Templates in another.

So this is kind of a great example of a common way of deploying IE configuration using both of these different policy areas to their best benefit.

---

## Managing (and Removing) IE Maintenance Policy

So let's shift gears now and talk about IE Maintenance Policy, which I mentioned at the beginning is being deprecated or has been deprecated by Microsoft.

If you're running either Windows 8 or above or you have a system running IE 10 and above, so that includes Windows 7 systems.

Now as I mentioned, IE Maintenance Policy has been around since the beginning and it has had issues in the past.

So what I want to do is talk a little bit about how it works, but then also dig into sort of how you can get rid of it.

So IE Maintenance Policy is per-user only, just like GP Preference policies, and it does allow you to configure many of the same things you can configure in GP Preferences Internet Settings.

And what's interesting is that there are some settings in IE Maintenance Policy that you can't configure in any of the two other areas that I've talked about.

Now the way IE Maintenance behavior works is when you're in the policy.

Lets say, I want to configure proxy settings through the Connection dialogue.

What it does is it actually imports the settings from the machine you're editing policy on.

So when you click on that Modify Settings button you see on the screen, it reads the Connections Settings configuration from your current IE browser settings.

And essentially, imports those in to the Group Policy object.

And this has been problematic in the past, because if you go to a different machine with a different configuration for, let's say, the configuration settings in IE.

And you click on that Modify Settings button, it then imports those new settings into your GPO.

Now, that was improved upon in later versions of IE Maintenance Policy, but there's still some versions where that behavior can exist.

And IE Maintenance settings kind of behave in a kind of quasi preference policy way.

So some of the settings will behave like preferences and the user will be able to modify them.

An example of that would be like home page settings.

Some settings will behave like policies and the user won't be able to modify them, but it will look like they can modify them.

So in other words, the interface won't be grayed out, but they'll be able to, and they'll be able to make a change to the setting.

But the next time they go in there or the next time policy is processed, the setting will be revert back to what is in the GPO.

So, it's kind of a, sort of a mixed bag with respect to how it controls the user's interface.

And this is another reason why, I think Microsoft has gotten rid of this policy area going forward, because it is kind of implemented in a sort of half, half baked sort of way.

So, you know, I think if I give any advice about IE Maintenance Policy, I would certainly not make any new IE Maintenance Policy going forward.

Given that it's going to be removed whenever you upgrade your browser past EI 9.

So, any time you're in IE 10, the settings and IE Maintenance Policy will no longer impact the browser.

No longer control the browser.

So the best way to deal with this is to remove it.

The probably the easiest way to do that is to simply unlink the GPOs that contain IE Maintenance Policy.

Assuming that's all that they contain or that the other things that they contain aren't important to you.

So not all settings are going to be undone, however when you.

Undo that or unlink that policy.

So, you'll probably end up needing to use either GP Preferences or Admin Templates to take over those settings that are not undone.

And that's where you can run into challenges, because if you have a setting in IE Maintenance policy that's not available in either of those other two areas, you'll run into issues where essentially, you don't have the ability to remove those settings easily.

Now, you can reset all the settings within IE Maintenance policy.

So, let's say you have a GPO that has IE Maintenance policy in it, but it also might have some Admin Template settings.

Some other settings that you need.

So, you don't want to unlink that GPO, but you can right-click on the IE Maintenance node and select the reset browser settings option, and that will essentially empty out the IE Maintenance settings within that GPO.

So, that gives you another way of sort of removing, or at least having the IE Maintenance settings no longer apply.

And then, you can go and do your cleanup.

---

## Managing (and Removing) IE Maintenance Policy

So now, I just want to illustrate how Windows IE Maintenance policy works, some of the, concepts that I just talked about.

So, lets go in under Internet Explorer Maintenance, and I'm on this Windows 7 system that has IE 9 on it.

So, I can still see IE Maintenance policy in the GP Editor.

And let's say I wanted to go in and make a proxy setting.

You'll note that it throws this message that says, settings you make on this page will import, will override imported settings from the connection settings page.

And I can go in and actually make changes to the proxy.

I can tell it to use the same for all of them.

Whatever happens to be, I can just set that on here.

If I go in under Connection Settings, this is where I was mentioning earlier, that it actually imports the current settings of this machine that I'm on, into the GPO.

So, if I come in under LAN Settings, these are the settings that it's finding on the machine.

And I can go ahead and set proxy.pac file, or whatever I want to do.

And these settings are now stored in the GPO.

So, a lot similar user experience to the GP Preferences area.

I can do things like set the browser title.

So whatever title I want will appear as the browser's window.

So I could say, you know, Internet Explorer from mycompany.com.

And that's something that you can't do in either GP Preferences or Admin Templates.

So, that's sort of unique to IE Maintenance policy.

I can add favorites to the browser, so I can push favorites to the user.

And that's another thing that I can't do with either of those other two areas.

Now, there are other ways to push favorites.

You can use GP Preferences shortcuts to, to send favorites to the, to the user's browser.

So that's, it's not this is sort of an old-school way of doing it, and the new-school way would be to just create URL shortcuts, essentially using GP Preferences shortcuts.

So, even though there are some things in IE Maintenance policy that perhaps you don't see, obviously, in the other areas things like favorites, URLs, those sorts of things are doable.

You just have to get a little bit more creative about them.

Now remember I mentioned that iIf you want to get rid of the settings in here, you can go ahead and reset the browser settings and it'll give you this message saying that it'll delete all the browser settings stored in the GPO and ask me if I want to continue.

And that once it's done, the, the browser or the GPO will essentially be devoid of all Internet Explorer Maintenance policy settings.

And again, you have to sort of manage, on the client, which of those settings remain behind, and which ones get removed, because not all of them will get removed.

Some of them will just get left or tattooed on to the system.

So overall, IE Maintenance policy really doesn't have anything we can't live without, and given that Microsoft is deprecating it, now would be a good time to get rid of it from your environment if you're already using it, and don't use it if you're not.

---

## Summary

So, to summarize this module, there are three ways that you can configure Internet Explorer using group policy.

There's Group Policy Preferences, IE Maintenance policy, and Admin Templates, and of course IE Maintenance has been around the longest.

And it has now been deprecated on any system where IE 10 is installed, and that includes all Windows 8 systems because they came with Windows with IE 11.

And any Windows 7 systems that have IE 10 or IE 11 installed on them.

So, what that essentially means is that you can no longer either edit or process IE Maintenance policy on those systems.

Now, GP Preferences lets you configure most IE options.

It, it does cover a lot of the things that IE Maintenance policy did, but does not enforce those settings.

So as, as I showed in my demo in this module, you can set the proxy setting, but the user can come along and change it, unless you do something about it.

And that something is Admin Templates, which does the sort of lockdown part of IE configuration.

It doesn't give you as many options for configuring IE as GP Preferences does, but it does enforce those settings.

So, for things like site-to-zone assignments, and for disabling the proxy screen and other things, the IE Admin Templates can be really helpful.

Now, you can use a combination of the two as I did in my demo, to get the biggest bang for your IE configuration buck, and I highly recommend that as your approach, because it just allows you to sort of mix and match the two.

To do lock down in cases where you need it or to just simply deliver preferred settings in cases where you don't.

Now, if you are using IE Maintenance policy, I highly recommend moving towards removing it or replacing it with a combination of the other two policy areas.

And I did show a couple different ways of removing the settings, either unkinking the GPO, or using the reset browser settings option on IE Maintenance policy.

Now, one thing to keep in mind is, make sure you keep around a system that can still edit IE Maintenance policy, because if you don't then you'll have, you'll be hard pressed to be able to actually remove it.

So it's important to keep at least one system running with IE 9 on Windows 7, for example, and that will allow you to continue to edit and potentially reset and remove those IE Maintenance policy settings.

---

