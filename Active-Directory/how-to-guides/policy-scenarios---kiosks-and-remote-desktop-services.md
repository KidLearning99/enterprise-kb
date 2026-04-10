# Policy Scenarios - Kiosks and Remote Desktop Services

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Kiosks and Remote Desktop Services |
| Type | Automated Transcript Synthesis |

## Understanding Group Policy Loopback Processing

So, as we start this module, I'm going to be looking at an interesting scenario which are, the deployment of kiosks and remote desktop services or VDI installations.

And how group policy can play in that kind of an environment.

So what I want to talk about is, the challenges around these particular sorts of configurations.

For example, kiosks, very, usually highly locked down.

They have, you know, running a specific application or a specific use, like, for example, you might have a computer running Windows in a public area like a library or an airport or someplace like that.

And you want to give access to a particular application, maybe it's to print your boarding pass or to do some specific activity.

So you want to give the user as little access to the system as possible.

You want to give them access to maybe a single application.

And, you know, having that kind of highly locked down configuration is something that, of course, Group Policy specializes in.

And kiosk mode in particular, or kiosks in particular are placed where Group Policy can really help out.

The other scenario where we see a lot of this kind of activity is in remote desktop server implementations or Citrix XenApp or even VDI systems where they're often systems, especially remote desktop servers, where they're being logged into by multiple users, often at the same time, running particular applications.

It could be Office, it could be the browser.

It could be any number of applications, and they're really all sharing a single OS in the case of remote desktop servers or Citrix XenApp.

In the case of VDI it's really more about sharing a single infrastructure whether it be computer, hard disk, and memory and you want those users to behave across all those VDI systems.

And what behave means is to perform activities in Windows that don't impact other people on that same shared infrastructure.

And you know, the other aspect of this is that they may only log into these RDS servers or these VT, VDI servers periodically so they may not actually be their primary machines.

They may be logging in when they're remote from the office or you know, over a VPN or just for a particular application that they need to get access to.

So, these kinds of sort of special use scenarios are another area where a particular aspect of Group Policy can be really helpful and that's called loopback processing.

And all of these scenarios really have one thing in common which is that the user's normal experience when they're logging into their Windows system is different depending upon that computer that they're logging into.

So if I'm on my regular desktop, and I go log into a kiosk machine, my experience has to be different on that kiosk machine.

And similarly, if I'm logged in to my regular desktop and I log in to a remote desktop server, or a XenApp server or a VDI system, again, my user experiences likely to need to be different.

And this is where this so-called Loopback Processing comes into play, and it's a special mode of Group Policy processing.

So, what is loopback? Well, it can be really confusing.

Whenever I try to explain it to folks, it's always a challenge to get the idea across the first time.

So I encourage you to look at this section again and again and I'll have some demos that'll show you sort of how it works.

But the idea is, it's meant to solve this problem that you need to deliver different Group Policy settings to the user depending on which machine they log into.

And really it's about categorizing these special role machines.

These special types of machines like kiosks or remote desktop terminal services as loopback-enabled machines.

And when they're loopback-enabled, the user, when they log into that machine, they're going to get a different set of user based Group Policy settings than they would get from a normal system that they're logged into.

So, let's just kind of graphically look at this, how this works.

So on the left hand side we've got the user in blue on their regular PC.

That little cog indicates their normal user settings that they get from the GPOs that are linked to their user account on AD.

And their computer gets those GPOs that are linked to the computer account in AD.

Now let's say this user goes and logs into this computer on the right, this green computer.

And this is a PC with loopback enabled, maybe it's a kiosk.

And it's getting its own set of computer and user GPOs.

And what happens in loopback mode is that the user then gets the settings that are specific for this computer and user in that loopback state or on that kiosk machine.

So the user's no longer getting their blue settings.

They're getting the green settings that have been delivered by the GPOs that are linked to the computer.

So this is kind of goes against all the previous discussions we've had about you know, GPOs with settings that are the computer's settings linked to the computer object and user settings linked to the user object.

In a case of loopback, that's sort of turned on its head.

And you can have user settings that apply to a computer that's in loopback mode and anyone that logs into that computer that's in loopback mode will get those user settings.

So, how do you enable this? Well, it's enabled on a per-computer basis and you just basically need to flip a switch on an admin template setting on a computer that you want to enable loopback for.

And it's this policy area here, under Group Policy, configure user loopback processing mode.

There's really two modes that loopback supports, one is called Merge mode and the other is called Replace mode.

Merge mode is where the user's regular policy settings that their regular user account in AD gets, those are processed first when the user logs onto a kiosk or a loopback system.

And then the user settings that apply to the loopback computer are processed second.

So, you end up getting two sets of user policy on this kiosk or loopback system.

Now, Replace mode is where user settings that apply to the loopback computer are processed first, and the user's regular settings are completely ignored.

So, they, basically, these kiosk or loopback user settings absolutely override or apply in place of the user's regular user settings.

So, let's get in and kind of dig into this a little bit and show you how this loopback processing is configured, and then we'll dive into some examples.

---

## Understanding Group Policy Loopback Processing

Okay, now, let's kind of walk through what it looks like to create a policy with loopback enabled.

So I've got a GPO, I call it the Kiosk GPO.

And I'm going to go ahead and edit it.

And if I come in under Computer Configuration > Policies > Admin Templates and System > Group Policy.

And what you'll see is if I come down here to configure user Group Policy loopback processing mode.

What I'll do is go ahead and enable this, and I can choose from Merge or Replace Mode.

For my scenario just as an example, I'm going to go ahead and use Replace Mode.

And now I've got this policy being applied at an OU I've called Kiosks.

And in the Kiosks OU, you'll see I have a Windows 7 client machine.

I might have multiple machines in here or even RDS servers.

I typically, and I'll talk about this in a little bit.

But I typically recommend segregating those.

kind of special use loop back machines into their own OU because it makes it a lot easier to manage those.

And now, now that I've got my Kiosk GPO and it's enabling loopback policy.

And oh, by the way, you don't need to have loopback policy the actual setting that I just set, set on anymore than one GPO.

So if you might have multiple GPOs linked to this kiosk's OU.

But you don't have to have loopback enabled on all of them.

That setting only needs to be processed once by the computer in order to get loopback policy.

So as long as it's enabled on this one GPO then I'm good.

For you know, any other GPO's that I want to link up to this OU.

So, I can either, there's a couple ways I can approach this, and I'll talk a little more about these scenarios in a bit.

But I can either create my user settings inside this Kiosk GPO or I can create separate GPOs.

That set those user settings as well.

So I could have, for example, I'm going to go ahead and create and link a New GPO that we can call it Kiosk User Settings.

And in here is where I'll set whatever settings I want on the user configuration side.

That I want to apply to users that log into these kiosk machines.

So, strictly on this side of the GPO, I can then start setting settings.

Like lock the task bar, various lock downs that I want to perform, I can do that on this Kiosk User Settings GPO.

And that guarantees that when a user logs into this Win7 client machine, that they're going to get.

Because we're in replace mode, they're going to get the settings that I specified here, instead of their normal settings from whatever OU they happen to be in normally.

---

## Kiosk Configurations and Loopback Processing

So now I want to talk about the configurations that you would use in a typical kiosk scenario.

And how you can use loopback processing in that scenario.

So, some of the characteristics of a kiosk and I mentioned some of these earlier but they're typically running a single or very few applications.

So it might just be, for example, the browser and nothing else.

And it's usually completely locked down, especially if these are public machines or public facing machines.

You don't want the users to be getting in and mucking around with the system, restarting it or bring up registry editor.

All kinds of other things that you pretty much don't want to expose to a regular user.

So the user can't do much in that typical kiosk system in a typical kiosk application.

And it may even use a different interface from Explorer.

So Windows has this capability of having custom user interfaces.

And so for example, a web kiosk may just be running a stripped down version of IE, running in kiosk mode.

And that's kind of a perfectly reasonable approach.

And it may have a dedicated Kiosk user account on that machine.

So it might be for example, auto logging on with the dedicated local account or even a domain account.

Or it just could be using the user's regular account.

So the user might walk up to a kiosk machine and log in with their normal account.

They're not going to obviously get their normal desktop, they're going to get whatever the kiosk configuration is set to using loopback policy.

So when running in loopback processing in the kind of kiosk scenario, I almost always see the replace mode of loopback used.

That's simply because Replace mode says, I want to replace all of your normal user settings with these special loopback settings that I've defined for the computer.

Whereas merge mode would apply both the kiosk user settings as well as the normal user settings that the user gets.

Which may result in a configuration that's not locked down at all.

So, there's lots of advantages in using replace mode in these kinds of environments.

So let's look at kind of a typical design for the kiosk configuration.

So you've got your Kiosk OU, and you saw that I created that in my Active Directory environment.

And it's got the one thing I didn't show was the OU has been set with the Block Inheritance flag.

And remember, I talked about that in an early module.

Block Inheritance says block all the upstream GPO's from applying to this computer or the user.

And the reason we do that is it allows us to ensure that the GPO's that are linked to this Kiosk OU are the only ones that are applying for these computers and users and it simplifies that the deployment of a Kiosk using loopback processing.

And then I've that loopback GPO the thing that enables loopback replace mode.

And again, it could be one GPO or it could be multiple GPOs.

But you can use that, those GPOs linked at the Kiosk OU to set those per computer or per user settings.

So, you have it all defined in one place, all of the computer settings that are required for the kiosk computers.

And all of the user settings that are required for users logging on to those kiosk computers are all set within this confined environment.

And that makes it really simple to ensure that you have the lockdown you're expecting to get.

Now as I mentioned, Kiosk typically locks down everything and may even deliver a different user shell from Explorer.

And you can do that using this custom user interface policy defined under user configuration admin templates.

It actually lets you set a custom shell for the user interface, and I'll show you that in a little bit.

Some other useful settings that you'll come across that you might want to consider.

Especially if you're not using a custom shell, are elements that lock down the user's desktop experience.

So, obvious ones would be.

This policy here for preventing access to the shutdown, restart, sleep and hibernate commands.

The last thing you want is somebody coming up to a machine and shutting it down.

So, you can remove access to those commands from this policy here.

And there are a number of other policies under this Start menu and Taskbar folder that allow for similar kinds of lockdown, and then finally another one is the prevent user from customizing their start screen.

If you're delivering a custom Start menu or if it's Windows 8.X and your delivering a custom Start screen.

The last thing you want is users rearranging icons or adding icons and this policy will allow you to, to sort of lock down that Start screen experience.

So now let's go ahead and deploy a kiosk configuration.

And I'll show you a little bit about what that looks like.

---

## Kiosk Configurations and Loopback Processing

So let's see how we can implement a kiosk using loopback policy.

So I've got this Kiosk User Settings GPO, that you see here.

And if I go up to my Kiosks OU, you'll remember that I have the Kiosk GPO that actually implements loopback policy, here.

And then I decided to break out my user settings that I want to deliver to the user, in this separate Kiosk User Settings GPO.

So if I go ahead and edit that GPO.

I'll show you some settings that I've made to try and lock down my kiosk.

The first and most important setting, is I've set the Custom User Interface Policy.

So, if I come in here under User Configuration > Admin Template System, the custom user interface lets me set a different shell from the Explorer shell.

And what you see here is that I've set the program files.

Internet Explorer, IE Explorer-k, now running IE Explorer that's the Internet Explorer browser with the -k option.

Puts IE into a kiosk mode.

It's a special mode of IE where all of the normal user controls are unavailable, and all you see is the browser.

So by specifying IE as the custom user interface, I'm essentially telling Windows not to run Explorer any more, but to run IE as the shell.

So I've got that set, and I've also made a couple of other system lockdowns, under start menu and taskbar.

I've locked the taskbar.

I've used that setting that I had in one of my previous slides to prevent access to the shutdown, restart, sleep, and hibernate commands.

And, I've enabled the policy to remove log off from the start menu.

So I don't want any semblance of giving the user the ability to get to or log off or shutdown the system.

Now the other thing that I've done is, under system, control alt delete options, there are a set of four policies that let me remove the change password lock computer task manager and log off policies from the control alt delete screen.

So the user is no longer able to perform activities like locking the computer, or logging off, or even changing their password from that screen.

So I've set all these policies up under the kiosk user settings GPO.

And I'm going to go ahead, and remember from a previous slide I talked about setting block inheritance on the kiosks OU.

I'm going to go ahead and do that so I don't get any superfluous upstream computer GPOs, or even user GPOs from, from any upstream GPOs like the default domain policy or the domain wide settings.

If either of those had computer or user policies in them, I would.

If I hadn't set block inheritance here, then my user logging into my kiosk machine would still get those.

Now one thing to note is, the user even though we're in replace mode here in loop back processing, the user will still get some of their normal user settings that aren't overridden by the kiosk user settings.

So in other words, the only time the kiosk user settings are going to replace the user's existing settings is if they are conflicting with or overriding the user's normal settings.

The user will still get some of their normal settings when they log into the kiosk.

But in a case of my configuration here, because I have set the custom user interface, none of those settings are really going to apply because all they're going to have access to is IE.

So, let's go ahead and log on to my Window 7 client and we'll see what happens when we do that.

I'm going to log in with my good old Joe Sales user account.

So now I'm logging in as my Joe Sales user account.

You'll see it's applying all of my normal user policies, but it also applied my kiosk loopback settings to the user in replace mode.

And what you're seeing coming up is essentially a user interface that no longer includes the Explorer, but rather includes IE in this kiosk mode.

And you'll notice that it doesn't have any windows or menu options on it.

It just has whatever home page I've specified here.

And if I try to, for example, do a control alt delete to get to the control alt delete screen, you'll notice all of my options are unavailable.

So essentially I've, thanks to policy, I've removed all those options I would normally have.

And now the user is really stuck in this mode where all they can do is browse the internet using this page.

And normally what you would do in a kiosk configuration is you'd set the home page to be something like your internet page, or whatever application website you want the user to be able to go to within this kiosk.

So it gives you that ability to lock down the user to doing a single function, and they really can't, thanks to policy, they really can't do much else.

So, that's in a nutshell, how using loopback processing can work in kiosk environments.

---

## RDS, VDI, and Loopback Processing

So, the other category of systems that I mentioned that benefit from loopback processing are these, so called remote desktop servers, terminal servers, citrix servers, and VDI or virtual desktop infrastructure deployments.

And where these are different from, you know, a typical desk top or a typical user scenario is that, for example, an RDS server might allow multiple users to log in and run applications on that server, and so the users are all sharing a single OS and essentially have to play nice and have to do behaviors that don't impact the resources against other users.

And so they need to be limited in terms of what they can do and that's where group policy and loopback processing can come in.

So in that scenario, you have loopback enabled on the RDS server or servers, and the user logs in and gets a different experience from their normal desktop when they log in.

Now, VDI machines are a little bit different because a VDI desktop is really just like a physical desktop except it just happens to be running in a virtual machine environment.

So there's typically only one user per machine.

But it also benefits from group policy limiting that user activity because, in a lot of VDI implementations, you get so called non persistent desktops.

And what those are are desktops where the users, the desktop is actually spun up when the user logs on and discarded when the user logs off.

So, you don't want the user to be able to, for example, save any user data persistently to that desktop, because it'll just be deleted when that machine, when they log off that machine.

So group policy can help here as well.

Some of the settings that you can use for these shared systems, from a computer perspective, the turn off, the system restore turn off, system restore policy.

What that does for you is it gives you, especially on VDI systems, it prevents activities like system restore, which really don't have any meaning in a VDI environment, and certainly not in a non persistent VDI environment.

Don't have any meaning to those systems, so you can turn off some of those features.

You can disallow offline files.

So what does is, let's say you've got users with folder redirection, their redirected folders are not automatically cached on that shared system.

So it might be an RDS server or it might be a VDI server, but you don't want the user's data to be cached there because over time, with lots and lots of users connecting or with those non persistent desktops in the VDI world going away, it doesn't make sense to cache files.

So this allows you to turn off that feature when the user is on one of these shared systems.

And then there's a whole slew of options under the remote desktop services, admin template settings, under remote desktop session host, that let you tweak and configure the remote desktop session as it's hosted by either an RDS server.

Or if you're using VDI with the Microsoft technology stack and you're using RDS to connect to a VDI instance, then these settings hold as well for that.

So, there's a whole bunch of settings to look at there.

I would also recommend using either GP preferences or security settings, system services, to turn off Windows Services that are not needed in these environments, and especially in VDI environments, I think this can come in handy.

So in addition, you've got some user policy settings that are useful in these shared system environments, and kind of related to the offline files are the Microsoft Outlook settings for turning off exchanged cached mode.

So again, just like offline files, you don't want a user logging into an RDS server or a VDI instance and having their entire Outlook inbox cached there.

It'll take up disk space if multiple users are doing it over time in the case of an RDS server, or in the case of a VDI server it may just get deleted when the user logs off so there's no value in it.

So I like to turn off, using this policy.

Now notice that this policy is a listed as a Microsoft Outlook 20xx.

I left the version number open there, because this is available in multiple versions of Outlook and Office.

But this does require the separate ADMX files from Microsoft Office that are available on the Microsoft download site, so you can get Office admin template files and add them to your GPOs as I've described in an earlier module in order to get access to these Outlook settings.

Another one that I like is personalizing and forcing the specific screensaver, so you don't want to let the user choose one of the 3D screensavers that ends up chewing up a bunch of CPU cycles on a shared machine, or on a VDI instance on a shared infrastructure.

You want to force like a blank screensaver, and so using this policy allows you to do that, allows you to get, basically force the user to use a specific screensaver that's not going to be impactful if it switches on.

So that kind of covers the two different scenarios where loopback processing comes in handy.

And now I'm going to summarize what we've learned in this module, and then in the next module, we're going to shift gears and go back to talking about how you can manage and take advantage of some of the management features in group policy.

---

## Summary

Okay, so now I want to summarize what we've learned in this module around loopback process.

So loopback processing again, is this notion that you can have a different set of user settings when a user logs on to a specific machine or machines than they would normally get from their user account.

So loopback processing comes in those two modes, Replace and Merge.

And remember that replace says we're going to run your user settings from the loopback machine and whatever user settings are processed by the loopback machine, but we're not going to run your normal user settings.

It doesn't mean it removes your normal user settings, it means it doesn't reinforce them from the policies that they normally get processed from.

So if your loopback user settings conflict with any of your normal user settings, those loopback settings are going to get, are going to win.

Now merge mode says first we're going to run your regular user settings just like we normally do, and then we're going to process the loopback user settings.

And again, if any loopback settings are in conflict with your regular user settings, they will get, they will essentially win.

So loopback settings for the user always win in the case of both merge and replace when there's a conflict with your current settings.

So loopback is useful of course for several different scenarios that we talked about, Kiosks, VDI systems, remote desktop server systems, Citrix XenApp or XenDesktop systems, any situation where you want the user to have a different user experience based on the particular machine that they're logging into.

Kiosks again, often run in a single application mode.

You'll recall that I set up a Kiosk that was running only IE, and the idea is to lock that system down to prevent all other user activity.

RDS system shared by multiple users.

VDI systems often wanting to be constrained, they certainly have shared infrastructure on the back end so you want to constrain what the user can do so that they don't wallop other users that are using that environment.

So both of these use cases benefit from loopback processing.

And again, you may end up using merge mode loopback because you still want the user to get their normal settings, but you might also want them to get some loopback settings.

---

