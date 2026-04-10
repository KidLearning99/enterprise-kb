# Group Policy's Capabilities

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Group Policy's Capabilities |
| Type | Automated Transcript Synthesis |

## What Can Group Policy Do

In this second module, we're going to talk about some of the capabilities of group policy.

So we, we've kind of laid the groundwork for what group policy is and how AD group policy differs from local group policy.

Now I want to talk about the not insubstantial number of things you can do with group policy.

It's really amazing, actually the numbers of policy areas that are available.

The numbers of things you can configure using group policy.

And sometimes, even as I have been doing group policy for years and years, sometimes I actually forget all of the things that group policy can do.

So, in this module, what I want to do is focus on kind of underlining those capabilities of group policy.

And giving you an idea about you know about what the things are that group policy is capable of.

And I don't think it's an overstatement to say that is does a lot.

That it's kind of the Swiss army knife of Windows configuration management.

Just like a Swiss army knife, there are lots of different gadgets that can do lots of different things.

There are per user settings, per user policy areas.

There are per computer policy areas, so things that effect a computer and all the users that log into that computer.

There's really something on the order of 40 different policy areas that you're, you're going to be dealing with or you may have need for as you kind of dig into group policy.

So this is a, as I said, not, not an insubstantial number of stuff that you can do.

So, let's, let's just kind of go through, and I'm going to, I'm going to really just kind of go through one by one and highlight each of the policy areas.

This is the, the only time in this, in this course that I'm going to do this.

And, you might find this a good reference going forward.

So that you have kind of a at least at a basic awareness of all the stuff that group policy can do.

So the first thing I'm going to talk about is Admin Templates.

This is going to be foundation technology I think I mentioned in module one that, Admin Templates came from the old system policy.

This is where you can set registry values that can apply to either a computer or user.

And the neat thing about Admin Templates and I'll talk more about this as I dive into this particular area later on in the course.

But Admin Templates do not tattoo the registry.

And what that means simply is that when a policy that contains an Admin Template setting is removed, the next time group policy is processed by a client system it will essentially remove the registry settings that were put in place by that policy.

Software installation, basically this gives you group policy based software deployment.

It's sort of I call it the poor man's system center configuration manager.

It gives you the ability to install MSI based set ups either per user or per computer.

And provide some pretty basic capabilities for deploying software.

It's good for simple packages.

I wouldn't necessarily consider it a replacement for SECM but I have seen some pretty aggressive implementations of software installation.

Security.

This is kind of another foundational area in group policy.

Security is everything from password policy to various security lock down settings to you know, anything you can imagine related to security.

It's kind of a general area that covers a lot of different stuff.

There's Wired Security.

And Wired Security is things like 802.1X.

So if you need to implement 802.1X authentication on a wired network, you can use Wired Security to set up the configuration of that, the authentication options for that.

Firewall and IP Security.

So this is where you can configure the Windows Firewall from group policy, add exceptions, add in-bound or out-bound rules, just control the general behavior of firewall as well as if you're using IPsec.

I don't see that implemented very much these days but if you do use IPsec that's a, that's a capability that you can configure.

Scripts.

Both per machine, that would be start up or shut down scripts, and per user log on and log off scripts.

Another very commonly used area.

Most people have been moving away from scripts towards the group policy preferences features.

But nonetheless scripts are still in use today.

Wireless security.

So this is, the 802.11 security configuration that you might want to set.

What, what SSID's a client should connect to, what security should be used for those, etc.

Folder redirection.

This is where per user folder, folders in user profile are redirected to server-based locations, typically.

So this is a way of kind of having data roam or follow a user, by putting it up on a server.

Software restriction and App Blocker.

These are two different technologies that essentially do the same thing.

They let you control the execution of various applications, with using fairly sophisticated rules, and I'll definitely be going into this later on in a future module.

Internet Explorer maintenance.

I have a little asterisk next to this, because Internet Explorer maintenance was developed to help you configure IE.

Do things like proxy settings and homepage, etc.

But it's actually been deprecated since IE 10 shipped and in Windows 8.

If you have either of those installed, either a Windows 8 machine, which of course comes with IE 11, or IE 10 installed on Windows 7.

There's no longer the capability to use IE maintenance.

Microsoft has sort of sunsetted it.

Policy based QOS.

This is for configuring network QOS on a system.

Not very frequently used but none the less there in group policy.

Deploy printers.

This gives you the ability to do per user printer mappings.

It's a very simple capability.

Lets you set a UNC path for a printer that gets mapped when the user logs in.

Network access protection lets you configure NAP.

Again, another kind of network security feature.

Advanced audit configuration lets you configure the audit categories that you want to be enabled for a given set of systems.

Name resolution.

This lets you control sort of the network names that a user or a computer can X to or, or can see in their browser.

Public key infrastructure X, X dot 509.

This is where you can configure certificate enrollment and other behaviors related to PKI.

And now we start getting into the group policy preferences areas.

I haven't talked about the differences between policies and preferences.

I'll get into that in a little bit.

But there's a whole new set, relatively new since since about Windows 2008 and Vista.

That Microsoft provided this group policy preferences feature.

And within the preferences there's a number of different extensions.

So environment variables is one that lets you set per computer, per user environment variables.

Users in groups lets you create users in groups on a local system.

Or even reset their passwords or do other things related to those groups and users.

Control membership, things like that.

Devices, lets you restrict which kinds of devices are allowed on a system.

USB removable devices can be controlled using this area.

Network Options, another area where you can control VPN configuration.

If you want a VPN connection widget to show up for a computer or user, you can do that with Network Options.

Files & Folders lets you distribute files or create folders on target systems.

Network Shares lets you create network shares on servers.

Data Sources lets you create ODBC data sources for users or computers.

Ini Files let's you modify the contents of an ini file.

Don't see too many ini files these days, but they are still in use by some so this is a way of being able to modify those.

Services lets you control Windows services configured on a system, not install services, but you can configure, you know, what the user account is that they use to log in, what their behavior is when they fail, that sort of behavior around services.

Folder Options lets you configure, if you go into Explorer under view Folder Options, there's a number of settings there you can actually configure those through policy.

Scheduled Tasks lets you create scheduled tasks on a system, so these are like jobs that can run and this is a way of distributing those scheduled jobs or scheduled tasks to a given set of systems.

Registry, this is sort of similar to admin templates, but not specific, not really behaving like admin templates do, this is kind of a free form registry, tweaking ability.

You can really go anywhere with this, configure any registry settings.

Printers, this is a, I'd consider this to be a more advanced form of the deployed printers.

GPP printers lets you do printer mappings and also lets you download drivers as a function of those mappings.

Shortcuts lets you distribute shortcuts to your users.

GPP-Power Options lets you manage the power management features in Windows.

Drive Maps, probably one of the most popular of the GP preferences areas, lets you map drives for users.

Start Menu lets you configure the behavior of the Start menu.

Regional Options controls that particular area within the control panel if you have different regions or cultures that you need to be able to configure, you can do that there, and then finally, Internet Options lets you configure IE and various versions of IE going all the way back to IE 5 and 6 and all the way up to IE 11.

So, per-computer or per-user.

Some policy areas are both per-computer and per-user, so some of the areas that I just talked about appear under both computer configuration and user configuration like Admin Templates, Security, GP Preferences, Drive Mappings, and Printers, but generally a setting that is per-computer will override the same per-user setting.

So if you have a per-computer setting under Admin Templates and the same setting under user, and typically, and it'll usually say in the explanation text that comes with the policy, the computer setting overrides the user setting.

So which one do you use? It really depends on your needs.

Per-computer means that every user that logs into a system gets the policy, so if you really need a setting to apply to all users on a particular machine, then given that setting that exists in both per-computer and per-user, you could definitely, preferentially configure it on the per-computer basis.

---

## Touring Group Policy Areas

Okay, so let's go ahead and bounce around inside of a group policy and see some of these policy areas that I just talked about.

I'm going to go ahead and start the GPMC, the group policy management console.

So, gpmc.msc is the command to start that, and when I do that, it will come up within my active directory domain, which is called r2test.tld, and if I expand the Group Policy Objects node, that will show me all of the GPOs that have been defined in my domain, and I'm going to go ahead and right-click this one called Test, and select Edit, and this takes me into the Group Policy Editor.

And, you'll notice of course, as I mentioned before, there's a Computer Configuration side, and a User Configuration side, and I'm going to go ahead and expand Policies under Computer Configuration, and going to Admin Templates down on to System, and I'm just going to expand my window a little bit here.

And you'll see here a number of policies and a number of folders if I drill into one of these folders, there are more policy settings, and essentially this is kind of a hierarchical folder structure within Admin Templates, where policy settings are available.

And I'm going to go back up to the System area and just click on one of these policy settings, and you'll see here in Admin Templates that all admin template settings have these three states not configured, enabled and disabled.

I'm going to go ahead and select Enabled here, and hit Apply.

That actually commits it to the GPO, makes the change to the GPO, and now I've just set an Admin Template setting.

Now some Admin Template settings also have the ability to have parts to them.

So if I click into one of these settings here, click Enabled, you'll notice that there are parts down here that provide additional options once I've enabled the policy, and in general, Admin Templates are kind of organized based on area.

So Control Panel, Network, Printers, Servers, et cetera.

Different areas include different capabilities.

These are all typically provided by Microsoft out of the box, and I'll talk a lot more about Admin Templates in a later module, but essentially you can actually customize the number of options that you see here to add new options.

Let me shift gears and talk a little bit about some Security Settings.

If I expand Windows settings under Computer Configuration, go into Security Settings.

You'll see here I have all of the security-related policies that are available within Group Policy, and as I think I mentioned, most of the security-related stuff is per-computer, because it applies to a computer regardless of whether there's a user logged in or not, that makes sense.

If I go into add Account Policies, and into Password Policy, this is where I can define password age, password length, et cetera.

Local Policies gives me access to a number of different things.

I can configure just basic auditing here, configure something called User Rights Assignment, which is essentially a list of rights that a particular user or group have on a system, and then, Security Options which are a set of, let's call them security behaviors that a system, that a Windows system has or can have.

And if I shift gears again, kind of rounding out my tour of some of these policy areas, if I expand the Preferences area, this is where all of the GP preferences are stored, and underneath Preferences there are two containers.

One's called Window Setting and the other's called Control Panel Settings, and under each there are a set of things that you can do.

And let me just go ahead and quickly create a new Environment Variable setting, and a lot of the Group Policy Preferences, UIs or interfaces look very similar to this.

So I'm going to go in here and I'm going to create a variable called test and give it a value of 1, and hit Apply, and I now have a new preference setting that will on next processing by a client, create a environment variable called test and give it a value of 1.

So this is just a kind of a quick tour of some of the many policy areas that I talked about in this module, and next we'll kind of dive in a little bit more around.

Some of the differences between policies and preferences and how settings are laid out within Group Policy objects.

---

## Windows Versions and Group Policy

All right, let's talk about how different versions of Windows interact with Group Policy.

And specifically when new versions of Windows ship, how Group Policy typically gets impacted.

So whenever there's a new version of Windows, and usually it's a major release of Windows, there are new policy areas that Microsoft introduces as I think I mentioned in Windows Vista and Server 2008.

The introduce the Group Policy preferences feature, which was actually technology they acquired from another company.

And they added in one release of Windows something like 14 or 15 different new policy areas.

But, usually, in the releases of Windows the improvements or the increases in policy areas is incremental.

So, Windows 7, they shipped maybe three or four new policy areas.

And in Windows 8, one, possibly? And usually where you see the main differences are with admin template settings.

So admin template settings are backed by what are called ADMX template files.

And those are just essentially text based XML files that drive the description of policies that you see under admin templates.

And don't worry about understanding a lot of that right now.

We'll get into more detail on ADMX in a little bit.

But essentially with each new version of Windows, you'll find new or updated ADMX files that represent policy control for new features of Windows.

So each major release, and even some minor releases like service packs, will introduce updated or new ADMX files.

Now, there's a good rule of thumb that you want to follow.

And that's that to edit a new policy area, in other words, that a policy area that's been introduced in, let's say, let's say Windows 8 comes along and introduces a new policy area.

That in order to edit that new policy area, you need at least the version of Windows that shipped that area.

So, what that means is, you, you want to have GPMC running on a Windows 8 machine in order to be able to edit the new policy area.

The earlier versions of Windows simply just don't see it.

Now, to process that new policy area the client needs to be running that new version of windows as well.

The client needs to know about that new policy area it has to have a extension client extension or client side extension.

Installed on the OS to know how to process the settings in the new policy area.

So generally, speaking that client needs to be running at least a version of Windows that shipped that feature.

So, what does this look like? So you've got a policy area in Windows 8.

In order to edit that policy area, you need Windows 8 with GPMC installed.

And in order to process that new policy, you need Windows 8 with the normal client side extensions installed.

And that's generally the case for any new policy areas that come along.

Now, with admin template settings, it's a little bit different because when Microsoft ships new ADMX templates, those files are not really a new policy area.

They're just new registry tweaks within a particular feature of Windows.

So you know, with Windows eight they added the metro ui or the modern UI and they added a bunch of new capabilities in the operating system.

That came along with some admin template settings that were specific to that modern UI.

And obviously in Windows 7, there is no modern UI, so what ends up happening is even if you define, Windows 8 modern UI admin template settings when you apply those to a Windows 7 machine.

They're simply ignored.

The registry tweaks are made on the Windows 7 machine.

But Windows 7 doesn't have anything, doesn't know what to do with them.

The UI is not there to be policy enabled.

So essentially, there's no harm in setting those in the admin template area for newer versions of the operating system.

The older versions will just simply ignore them.

So let's do a quick demo to sort of illustrate how this works.

---

## OS Version-Specific Policies

Okay now, let's look at some of these differences that operating system versions introduce within group policy.

Now, I'm on my server 2012 R2 box and that's essentially equivalent to a Windows 8.1 client operating system.

And very much, it's very much the case with group policy that the server side operating systems, like in this case server 2012 R2 and the client side are synchronized with respect to Group Policy.

So if a group policy area exists on Windows 8.1, then it's also going to exist on Server 2012 R2.

Now, where you'll find differences is in admin templates, where there might be some server-specific admin templates on the server's queue on 2012 R2 that don't appear in GP Editor if it was launched on Windows 8.1.

In any case, let's go ahead and bring up the GPMC.

And I'm going to go ahead and right-click and edit the test GPO.

And I'm just going to take a look at a couple different areas.

So, on one area, I'm going to drill into admin templates per computer.

And let's look under Windows Components, and you are going to see a set of subfolders for various features that are enabled through policy.

And one of them is App Runtime.

Now, let's make note of that, because what I am going to do now is come over to a Windows 7 system.

And I'm in GP Editor, also on the test GPO.

And I'm going to go down to the Windows Component section under Computer Configuration.

And you'll notice there is no App Runtime folder.

And this is simply because that feature or those policies that are specific to the app run time are simply not in Windows 7.

And so they're not implemented as ADMX files, or admin template files on a Windows 7 system, which I'm on right now.

And therefore I don't see them.

Now I'm in both machines, I've got the test GPO open.

So if I were to make a change to the test GPO on this Windows 7 machine and then close GP Editor.

And make a change to the test GPO on the Windows 2012 R2 machine.

Both are valid changes and both could apply equally to either Windows 7 or Windows 8 or server 2012 machines.

The point there is that if there are settings that I define on the Windows 8 machine, for example, some of those policies under app run time.

They will simply be ignored by the Windows 7 machine that processes them.

Let's take another example of this difference between operating systems.

If I go into User Configuration Preferences and I look at Internet Settings, this is where I can configure Internet Explorer settings using GP preferences.

You'll notice that I have Internet Explorer 5 through Internet Explorer 8 available to configure on Windows 7.

Now, if I go back to my ter, server 2012 R2 machine and I go ahead and expand Preferences > Control Panel Settings.

And right-click Internet Settings and select New, you'll notice that I now have up to IE 10 available.

And that's simply, because Microsoft added that capability in the newer versions of the operating system and those capabilities obviously don't appear in Windows 7.

But if I defined an Internet Explorer 10 policy on my Windows 2012 R2 machine here and applied it to a Windows 7 system, it would actually process.

If that Windows 7 system had IE 10 installed, it would actually process those settings just fine.

So there are some cases where newer versions of the operating system can be taken advantage of to provide controls on additional features within lower level versions of the operating system.

So this will become a little bit more clear as we dive into some of the scenarios.

But the point here is that newer versions of the OS implement new features.

In some cases, the lower level versions or down level versions of the OS, can take advantage of those and in some they can't and it really ends up depending on the scenario and the setting.

---

## Policy vs. Preferences

So now, let's talk about the differences between Policies and Preferences.

I've talked kind of loosely about these two different areas and I've sort of delineated them artificially, but there are some significant differences in how they behave and how the clients that process policies behave as compared to preferences.

So what is a policy and what is a preference? You know, that there's been a long history of terminology confusion here.

Unfortunately, Microsoft has sort of done this to themselves.

Before the group policy preferences feature shipped, they used to call any Admin Templates that were not enforced.

In other words, that could be changed by the user, they used to call those preferences.

And that went back, that had kind of a long history going back to system policy.

But since group policy preferences was introduced that definition has sort of gone by the wayside.

And now when we talk about the difference between policies and preferences, generally policies are things that cannot be undone.

Whereas preferences can be, unless they're otherwise restricted.

And what I mean by that is that you might have a preference, for example, that sets the IE home page to Google.com, but the user can go in and change that just fine, unless you come in and follow it up with a policy that prevents the user from changing the home page settings on IE.

So you know, using a combination of preferences and policy there, you sort of can make preferences look like policy.

But at the end of the day, preferences can generally be undone and policies cannot.

So think of preferences as kind of suggestions, you make a change in configuration of a system for a computer for a user and the user can change it if they want.

But by and large that has been enabled and is there.

Whereas policies are really commands and the commands are enforced by the applications that consume the policy and by the security of the operating system.

So, in the case of a policy that removes the RUN command from the Start menu.

If the user goes in and selects the Start menu on a Windows 7 machine and is looking for their RUN command and it's been basically turned off by policy, they cannot bring it back.

It's simply unavailable and policy has commanded that it not be there.

So, it's really about on the one hand, suggesting that the operating system look a certain way versus on the other hand commanding that it looks a certain way.

So how does this all work? How does policy work? Policy does require that the OS and the applications and processes being controlled, so the things that are being controlled by the policy are aware of that policy.

In other words, the application developer that wrote that application or process, even if it's the explorer shell or Internet Explorer or Microsoft Word.

They wrote those applications to look for specific policy settings.

In this case, registry settings.

And in the presence of those registry settings, the application behavior changes.

Usually, dialogue gets grayed out or removed or set to a certain value and then grayed out, so that the user can't change it.

And that's typically how a lot of these policies work.

So in this example on the screen, I've enabled the disable changing home page settings policy.

This is a policy under Admin Templates.

And now that I've enabled it, if I go and look at the client that processes it, you'll notice that if I bring up Internet Options in Internet Explorer that the home page area is completely grayed out.

Not only can I not type in a different home page, but I can't use any of the buttons associated with the home page, they're all grayed out.

And this is a function of the policy that I set on the left-hand side.

Now preference on the other hand is really just modifying existing configuration settings, be they registry settings or configuration files or, you know, database settings, whatever it happens to be on the operating system.

Preference is just modifying an already existing setting, so there are no previously made lockdowns or changes of, within the application to do something in particular.

These are just configuration settings that the application came with by default and the behavior of the app doesn't change when they're set.

It doesn't get grayed out or it just typically gets the value filled in.

So in this example, I have a GP preferences Internet settings preference that's setting the homepage to GPOGuy.com.

And when this gets processed by a client, in this case, a Windows 7 client.

You'll notice that the home page has been set to GPOGuy.com.

But I can go in and change that it's not grayed out and all of the buttons below it are also not grayed out.

So essentially, the home page has been changed in IE, but I can change it back to something else whenever I want.

This is kind of the main difference between policies and preferences is that one requires the application to be aware of those policies and modifies its behavior accordingly.

Whereas preferences just sort of works within the system, so to speak to change the default settings or configuration values for a particular application or setting.

So of course, there are always exceptions to the rules.

Preferences that modify the areas of the OS the user doesn't have native OS rights to are can be just as commanding as policies.

So what that means is if I set a per computer GP preference to create a local user group on the work station on a given work station.

And as a user, logging into that work station, I don't have the rights to modify the local user accounts, delete them.

Modify their properties.

I'm not going to be able to change that preference that was put in place.

So preferences still respect the native security that's built-in to the operating system but they don't, so they can sometimes act like policies.

But in that case, the application is not behaving any differently than it normally would, it's just respecting the operating systems security model.

And then by the same token policies that are set using set just setting regular old registry values, and not you know, not a specific policy based registries that have been enabled by the application, or that the application has been made aware of.

Those are just as suggesting, or suggestive I guess is as preferences.

So, if I, if I create a, 80 MX file under admin templates that just goes in and sets the home page in IE, I don't necessarily enforce that.

It won't be forced onto the users that process that, they can still go in and change it.

So it's a little bit of a kind of a subtle nuance and I'll definitely dig into this a little bit more but the bottom line is that most policies force themselves upon the user are computer and most preferences suggest.

And again, it really all depends on what the setting is and who has the rights to change it.

So, the final point here is that, you know, if you really want to enforce policy, make sure that a policy can't be undone, then you really don't want to make your users admins on their work stations.

I know that this is a fairly common practice, but it's a really bad practice if you care about using policy, if you want policy to stay in effect.

And I'm going to reiterate this point throughout the course because it's an important one.

So just to repeat.

If you make your user an admin on their workstation, they can undo any policy that you put in place.

It doesn't matter how enforced it is by the application, they can go ahead and undo it.

And that's just a function of the operating systems security model and the fact that administrators have rights to the entire system.

So let's jump int a quick demo that kind of illustrates some of these concepts.

---

## Policies and Preferences

Okay, so what I want to do in this demo is show you the effects of different policies and preferences.

So I went ahead and created a GPO called policy and preference in my server 2012 active directory domain.

And I'm going to go ahead and drill into the GPO editor here and I'm under User Configuration, Policies, Administrative Templates, Windows Components, Internet Explorer and you'll see here the Disable change home page settings policy.

So let's go ahead and enable that.

And I'm going to go ahead and set the home page to Google.

Currently, it's set to Bing for the user that's going to get this policy.

So now that policy is enabled.

And, again, this is a policy, not a preference.

So, let's go ahead and switch to the client.

And here's my Win 7 machine that's got IE 11 up with the Bing home page.

And if I go up into Internet Options, you'll see that Bing is indeed set to my home page.

Let's go ahead and close IE.

I'm going to go ahead and bring up the command prompt, and when I bring up the command prompt I'm just going to say gpupdate.

This tells the client to go fetch the latest policy that applies.

And essentially, what's happening here is that it's getting that policy that I just created which has been linked to the OU that my user account is in.

And now if I bring up IE, you see that the home page is now Google.

And if I go into the Internet Options, this is all grayed out because I had set this home page in a policy.

Okay, now, let's shift gears.

I'm going to go ahead and set that back to Not Configured.

That means that the user will no longer get the policy that's enforcing that particular home page setting.

And I'm going to go into Control Panel Settings under Preferences for the user, create a new IE 10 preference.

And I'm going to set this to gpoguide.com.

And now, I'm going to tell it to start with the homepage, go ahead and apply that policy.

Now I've got my Internet Explorer 10 and this also covers IE 11 as well.

Now I've got that pol, that actually preference set.

So this is a preference.

So now I'm going to come back to my client, go ahead and quit that, and do my GP update.

And let it update the policy for both the user and the computer.

In this case, I'm only looking for user policy updates, but the computer policy updates happen anyway.

And now when I bring up IE, you'll see that it goes to gpoguy.com, and if I go in under Internet Options, the home page is set.

But I can go ahead and change this, I can change it back to Google.

And nothing is going to stop me from doing that because this is a preference.

And, then the next time I bring up IE, I'm back to Google.

So, the only thing that will cause that to go back to gpoguy.com is if the preference is forced back down through policy or through group policy processing.

So essentially, preferences give you a way of suggesting things to the users.

Policies give you a way of commanding things to your users or computers.

And and this is sort of the the easiest way you can show the differences between the two, is by picking something obvious like IE home page, show how you can lock it down, and then show how you can just suggest a new home page for the user.

---

## Summary

Okay, let's wrap up this module by summarizing what we learned.

Group policy contains nearly 40 different policy areas doing everything from drive mappings to printer mappings, to registry settings to security.

The list is, fairly long as we showed.

Some of these are for only for the computer, some are only for the user, and some cover both computer and user.

Typically new policy areas will ship with a major release of Windows or a service pack, some kind of update that happens, significant update that happens to Windows.

And when that policy area ships, that new policy area, you'll typically need to be running at least that new version of Windows to edit the new policy area, to edit the settings within it, and at least that new version of Windows to process it.

So if for example, Windows 8, ships a new policy area, you'll need a Windows 8 machine with GPMC installed to edit and create new policy settings within that new area, and then a Windows 8 machine to actually process it.

And, as I mentioned, if you try deploying those Windows 8 policy settings, those new policy settings to a Windows 7 machine in all cases, it'll simply ignore it if it doesn't have that feature installed.

And then finally, policies are different than preferences.

They are two different kinds of things that group policy can do.

Policies can generally not be undone by the user, and they are typically enabled by the applications in processes that, drive them.

So for example, if the Windows Explorer shell, wants to have the run command removed from the start menu, Microsoft has to code the explorer to respect the registry value that the policy sets that removes that.

On the other hand, preferences are really suggestions.

They are configurations to systems, applications, processes that don't prevent the user from going in and changing them after they're set but do set kind of the default configuration for those for those areas.

And the user, if they have the rights, can go in and change them.

So that wraps up the second module.

In the third module, we're going to dive into group policy processing and help understand how policy is actually processed at the client.

---

