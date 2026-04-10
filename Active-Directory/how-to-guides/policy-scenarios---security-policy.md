# Policy Scenarios - Security Policy

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Security Policy |
| Type | Automated Transcript Synthesis |

## Overview of Available Security Policy

So in the last module, we talked about how administrative templates policy can help with lock down of the user environment, the desktop environment.

In this module, I'm going to shift gears and talk a little bit about how security policy can help with sort of lock down of the system itself.

So let's dig in and see what that's all about.

Security policy is really not one thing.

It's composed of a lot of different policy areas under ComputerConfiguration\Policies\WindowsSettings\SecuritySettings.

There are a few settings under User Configuration in this same path, but by and large, the majority of security policy is configured on computers rather than on users.

And there's a lot of stuff you can do with security policies.

So for example, Account Password and Lockout Policy.

This can be set for the domain or for local user accounts on work stations and servers.

Audit Policy, configuring basic auditing, what is audited on a particular Windows system.

User Rights Assignment, who can do what on a system.

So, who can log on locally, who can log on from the network, who can perform certain tasks.

This is all controlled through User Rights Assignment.

Security Options.

I like to call these the kind of the vulnerability switches, so various default behaviors in terms of the Windows security subsystem and how it behaves.

Things like user account control, whether you have full prompts or minimal prompts, et cetera.

That can all be configured through security options.

Event Log Size and Retention Configurations.

So how big should the event logs grow and what happens when they fill up? Restricted Groups.

I'm going to demo this in a little bit, but this is a feature that lets you control local group membership on Windows servers and desktops.

System Service Security and Startup.

So you can control who can modify the configuration of a Windows service on a system and what the default startup behavior should be.

Should it be running automatic, should it be running disabled? Those kinds of features.

Registry and File System Permissions.

So you can actually set the permissions on registry keys and files in the file system using group policy, and this can be very powerful.

You can make mass changes to file system security using group policy.

Wired and Wireless Network Security.

So whether you're on a wired network using 802.1X authentication or on a wireless network that requires specific passwords for SSIDs and different authentication schemes.

You can set options for those within this area.

Windows Firewall and IPSec Configuration.

So, Windows firewall is probably one of the more common security policy areas in group policy, and this allows you to do very fine grain control over the rules for inbound and outbound traffic on work stations and servers.

PKI, or Public Key Infrastructure Policy, lets you set behavior for certificate usage on workstations and servers.

AppLocker, this is another feature I'm going to go into in a little bit.

AppLocker lets you control which applications can execute on a system.

You have really fine grain control over what code runs on a particular work station or server, and this can be really beneficial in terms of controlling malware.

Network Access Protection and Network List Configuration.

So network access protection, or NAP, can be configured.

The Microsoft NAP solution can be configured through group policy as can network lists, which are essentially the things that the user sees when they're browsing their network.

And then, Advanced Audit Configuration.

This is the, kind of a follow on capability from the basic audit policy, that allows you to set subcategories of auditing at a more granular level.

So, lots of power, here, in terms of what you can do with security policy.

So let's talk about some tips for deploying security policy.

By and large, most security policy will tattoo a system.

So unlike administrative templates, when you apply a particular security setting, let's say it's file system security or registry permissions.

And you remove the policy that deployed that, the permissions are not going away.

They're staying on the system.

And this really makes sense, because you don't necessarily know what the previous state of some security settings are, unless it's somehow kept or recorded.

So most security policy will tattoo a system.

And then by and large, most security policy will not merge settings from different GPOs.

So as an example of this, if I have restricted groups policy that's controlling the local administrators group, and I have that defined in a GPO linked at the domain level.

And then I define at an OU level a restricted group's policy that's setting the local administrator's membership, and I'm defining different groups in that.

The result that the computer at that OU level sees is not the amalgam of both of those, but really, going back to our LSDOU processing order last writer wins, so whatever settings are defined in restricted groups at the OU level will be the ones that win.

Account Policy, so Password, Lockout, Kerberos must be defined on a GPO linked at the domain-level in order for it to affect AD accounts.

This is a long-misunderstood concept within group policy.

In group policy, there is only one way to set domain.

User account policy, in other words, password minimum length, password, how long a password lasts, 90 days, 30 days, whatever, whether a password is locked out after a certain number of unsuccessful attempts.

All of that can be, or must be, defined at the domain-level in order for it to effect AD users accounts, user accounts.

Now you can define account policies at lower levels in the hierarchy, but it's only going to affect local user accounts on those machines that process that policy.

So, it's not for domain accounts at that level.

Now, Microsoft did introduce this thing called fine grained password policy, which is completely outside of the realm of group policy, that lets you set more granular account policy on a per OU or per security group level.

This has absolutely nothing to do with what I'm talking about here with domain account policy.

There can be only, in the group policy world, there can be only one account policy defined for a given domain, and it has to be in a GPL linked at the domain-level.

And just for the sake of discussion, what I typically recommend is the default domain policy, which is one of the two default policies that Microsoft ships in AD, is a good place to put account policy, password policy, lockout, et cetera.

I would also say that you should consider applying different security lockdown from servers to desktops.

The requirements are often very different.

And I think it's important to keep them separate so that you have dedicated GPOs setting security on desktops and dedicated GPOs setting security on servers.

And then finally, kind of hearkening back to my best practices for admin template deployment, I recommend layering security into GPOs.

So you have a base OS hardening GPO, so to speak, that does the base OS hardening, security for things like who can log on and what the basic permissions are on a particular system.

And then, you layer on.

Additional security hardening for whatever workloads or applications are running on those systems.

So if you have a server that's running IIS, it has additional hardening that you might want to do specific to IIS.

And that would be implemented in an IIS hardening GPO that's applied separate from the base OS.

And that way you can reuse the base OS security for many different servers and then layer on whatever that roll the server plays.

---

## Deployment Scenario Group Membership Control

Okay, now I want to talk about some deployment scenarios using security policy.

And, the first one I'm going to go through is group membership control which is a pretty common scenario in a lot of situations.

Whether you're talking about controlling membership on desktops or servers, there are two ways to enforce that local group membership.

And again, I'm going to reiterate that group policy does local group membership control.

It does not do anything with respect to groups that are defined in active directory, and that's a key distinction.

You'll need other mechanisms besides group policy to control membership on active directory groups.

But let's look at the ways that we can do this in group policy for local groups.

So the first way is restricted groups policy.

So you have the ability through restricted groups to do a couple of different kinds of control which I'll talk about in a second and that gives you the ability to essentially pretty much locked down the membership of the group and also which groups are in other groups, so adding groups to groups or adding users to groups.

The other way is actually through preferences, through group policy preferences, and I mentioned or alluded to this that even though group policy preferences has this preferences name in it, it is capable of doing lockdown if the thing that you're controlling is generally not accessible to the normal user.

So for example, group membership.

If the user is not a local administrator on their workstation, they cannot adjust group membership, no matter how it's delivered to them, whether it's through restrictive groups or through this Control Panel Settings Local Users and Groups policy.

So, these two mechanisms provide similar features.

I'll talk about where they're different, what the differences are, and in terms of, you know, controlling group membership on workstations and servers.

So restricted groups allows you to create list of users or domain groups that you wish to be added as a member of a local group.

So here's a scenario for this.

The workstation, Windows workstation and member servers have a local group that's built-in called remote desktop users, and members of this group can remote desktop to a server workstation.

Now, you might decide that you want your help desk users in active directory to be able to remote desktop to any desktop in your organization.

So, you can imagine creating a restricted groups policy that adds the help desk group, if there is such a thing in your domain, to the local remote desktop users group.

And that is perfectly perfectly something you can do with restricted groups.

The other thing you can do with restricted groups is have absolute control over the membership of a given group.

And in that scenario, it will actually dynamically remove users and groups that are not in your so-called approved list east, each time GP processes.

So this is kind of cool and actually somewhat dangerous.

In that you can really shoot yourself in the foot if you don't build your group memberships correctly.

But the point here is that the this particular mode of restricted groups has the ability to very tightly define group membership.

Now the members of this group side of restricted groups, there's two boxes that you saw in that previous screen, the members of this group side are about absolute control, that I was just talking about.

The group is a member of side, is where you would add users and groups to other groups.

So, you have to be careful when you're defining this and I'm going to go through this in a demo, which side your defining membership on, because one side is absolute control and the other side is sort of discretionary control.

Now with GP preferences Local Users and Groups, it's a little bit different.

So with this, you have a way to essentially add users and groups to another group, and that's really the main capability that this feature provides.

But it also lets you rename that local group or add a description to it, you can delete all member users, or all member groups from a group.

So this is kind of a almost similar to the absolute control power except that it's just going in through and sweeping through all member users and all member groups to clear out the group.

And then you can add or remove specific members to or from a group.

So I can say, for example that my help desk admins group will be added to the remote desktop users group.

But Joe Smith, who was an admin that I have that's no longer with the company, and has his credentials spread all over these local groups, will be removed from that group.

And that's an easy scenario to handle with the GP preferences local users and groups feature.

Now even though it's a preference, users cannot override these as I mentioned, unless they're a member of local administrators on their workstations or their server.

This is just as good as any other policy because the operating system security protects against tampering of this kind of group membership stuff.

So really cool capabilities around group membership control, local group membership control, and group policy, and what I'm going to do next is give you a little bit of demo as to how this works.

---

## Group Membership Control

Okay, now I want to demo this restricted groups, or group membership control scenario, using restricted groups policy.

So I'm on my Windows 7 desktop machine that's a member of the marketing OU.

And I'm looking at the local groups on this Windows 7 machine.

And I've got this remote desktop user's group, and I mentioned it earlier as the group that grants remote desktop access to the system.

And you'll see that it has no members in it.

So then what I'm going to do is come over to my domain.

And I'm going to go ahead and create and link a new GPO in the domain.

I'm going to call it the Restricted Groups Policy.

And now that I have this linked and defined on the domain, I'm going to go ahead and edit it.

And I'm going to drill in under Computer Configuration > Windows Settings > Security Settings.

And you'll see here I've got Restricted Groups as a policy area.

I'm going to add a group.

Now, I've got a group called Help Desk Admins that I'm going to use as my policy target.

So, if I click on Check Names, it'll return Help Desk Admins that's a global group I created in active directory.

And then, what you'll see here is that when it comes up, I've got two options.

I can either use members of this group, which is the absolute control portion of restricted groups policy.

Or I can use this group is a member of, which lets me add the target group, in this case Help Desk Admins, to other groups, and that's what I want to use.

I don't want the absolute control part, I want the discretionary addition of groups part.

And what I'm going to do here is, I'm going to say.

Add it to Remote Desktop.

I'll search on that name.

Add it to Remote Desktop Users.

Now this group, meaning Help Desk Admins, will be made a member of Remote Desktop Users on all the computers that process this policy.

All right, so now I've got my member of, for Restricted Groups set.

Let's come back to my client.

We'll go ahead and fire-up gpupdate.

Let's open a command prompt, gpupdate /target:computer because I'm interested in computer policy.

And policy getting applied to this workstation.

Now, let's go in under Remote Desktop Users and see what happens? There we go.

There's my Help Desk Admins group that's now been added to this Remote Desktop Users group.

So a quick and easy way to use Restricted Groups policy to control local group membership, and, again, if I were not an admin on this workstation, I couldn't go in and change this group membership at all.

So, essentially, I'm enforcing this policy over my machine's configuration.

---

## Deployment Scenario Managing Windows Firewall

The next scenario that I want to talk about is Managing Windows Firewall.

So let's dive into that.

Group Policy provides Windows Firewall with Advanced Security under this Security Settings path within Computer Configuration.

The old way of configuring Windows Firewall was actually within Admin Templates.

So if you had a Windows XP or Server 2003 client, there was a section under Admin Templates for the computer that let you configure this.

That has been superseded by Windows Firewall with Advanced Security.

That lets you configure the Domain, Private and Public Profile states for the Windows Firewall, whether it's on or off.

It also lets you configure inbound rules, in other words, to what traffic is allowed inbound to machines that process this firewall policy and outbound rules now.

So you can set rules on what traffic can go outbound from a machine, which has interesting security implications and also challenges as well as IPsec Rules.

IPsec is a sort of tunneling protocol that allows you to securely traverse networks and provide tunnels for encrypted traffic, and Windows Firewall with Advanced Security is your tool for configuring both Firewall and IPSec.

In this particular policy area, those IPSec rules are called Connection rules.

Windows Firewall Profiles which I talked about in the previous slide are what you can use to have different firewall behaviors, depending on what kind of network you're on.

So the 3 firewall profiles that are represented in the policy are, Domain, Private and Public.

Domain being when you're on the corporate network in other words you have a domain controller that's accessible to the client.

Private meaning you're on your home network which means it's a trusted network but it's not your work network.

And then Public is pretty much everything else it's when you're in a cafe, or on an airplane or in an airport and you're on essentially what is a the wild west for wireless networks.

You can configure firewall on or off for each of those profiles, as well as other postures based on each of those different locations that you can be in.

This is really a good idea especially for public networks.

If you do no other configuration of Windows Firewall, do one for Public Profile so that when your domain join machines go off network.

Laptops and such are connecting at Starbucks, that you are controlling what inbound traffic is able to get to those machines.

Let's talk a little bit more about Inbound and Outbound Rules.

Inbound rules are definitely the most common.

This is what we had before Windows Firewall with Advanced Security in the XP days.

It's basically saying who can talk to this system, who can initiate traffic from externally to this, to this particular system that's processing policy.

Outbound rules are more interesting and more challenging because it, it kind of assumes that you know all of the outbound traffic going from a workstation to other sources on your network.

This is definitely easier to do if you're on a server with a pre-determined workload instead of applications and you can more easily characterize the traffic of those applications to, from the server to external sources.

But it's a lot harder to do in a workstation world where you've got users running different applications all the time that may have different protocol and port needs.

So, I think, you know, outbound rules are one of those thing where, if you're going to go down the road of Host-Based Firewall Configuration, outbound rules are sort of the next level of sophistication in terms of doing that.

That rules can be set to be either allow or deny.

Once the firewall is turned on for a given profile, in other words, let's say on the Domain Profile that the firewall is on, then all traffic is implicitly denied inbound but not outbound unless exceptions are defined.

That's important to understand that the default state when the firewall is on is to deny all.

And you can create exception rules that allow specific traffic in, but by large everything else is denied.

This is just a kind of a UI that talks about how you create a rule, and I'm going to show you this in a demo in the next session.

You define the thing that you want to block, and it could be a Program, a Port, Predefined or Custom rule.

You say whether it's allowed, in other words, the action decides whether it's allow or deny.

And you tell it which Profile the rule applies to.

Let's go ahead and dive in and do a demo of this Windows Firewall capability, so you can see sort of how it works.

---

## Managing Windows Firewall

Okay.

Now what we're going to do is we're going to go ahead and do some Windows Firewall policy.

I've got a GPO that I created on the sales clients OU.

I'm going to go ahead and Edit that GPO and drill into Computer Configuration policies, Windows Settings, Security Settings.

And you'll see here, Windows Firewall with Advanced Security.

And if I click on this node here, what it's going to do is show me this screen that I can then set various states of the Windows Firewall, Connection Security or IPsec Rules and then my Inbound and Outbound Rules.

The first thing I'm going to do is go into Windows Firewall Policy, and this is where I can turn On or Off firewall based on current state so for the Domain Profile what I'm going to do is I'm going to say turn it On so the Windows Firewall will be on when I'm on the corporate network.

Now you'll see here for Inbound connections the default is to Block inbound connections.

The default is to Allow outbound connections.

So I'm not going to change any of the defaults.

And I can also do things like Customize the firewall notifications, whether or not I allow Unicast response to a multicast or broadcast, and allow merging of rules so that if I've got.

Rules that have been set across multiple GPOs or within the local firewall.

Then I can merge the domain firewall rules with local firewall rules rather than overriding them.

So anyway, let me go ahead and just set my default Domain profile to be On.

And I'm going to set the Public profile to be On, as well.

Now that those two are on, I've got Windows Firewall for the Public and Domain profiles turned on.

I can go ahead and define some inbound rules.

Now remember for the Domain and Public profiles with the Windows Firewall on, that means no traffic can get inbound.

So what I want to do here is, is create a new Inbound Rule.

And I can either use, define a particular program that is allowed to connect inbound, a particular port, or I can do predefined services that Microsoft has provided or custom rules.

And I like the predefined services because it's got a lot of things that, I would normally want to do with a system, covered here.

So for example, Windows Management Instrumentation, WMI.

I'd like to be able to do remote WMI management against workstations or servers that process this policy.

So I'm going to create an exception, for Windows Management Instrumentation.

And you see that, it actually creates three different rules for this particular, exception that I've chosen.

And I'm going to allow the connection.

And now I have for all profiles, I now have these three rules created.

Now if I wanted to be selective about this, I could say, so apply this to only some of the Systems that I'm interested in.

I could do Advanced, so I could say here un, under the Advanced tab, I can select.

Well I only want this to apply when I'm on the domain.

I don't want to open up WMI for users that are on public networks.

So go ahead and specify it only for the domain profile.

And I can do that for this one as well.

So all of them are the same.

And now, I've created an exception for the default inbound profile.

That allows WMI traffic to get through.

And that's really as simple as it is for creating rules and configuring the firewall.

And you can you know, set these in multiple GPOs, or you can define them all in a single GPO.

Which I actually, I typically recommend keeping all of your firewall settings together.

Unless you have lots of different kinds of rules that you need to apply.

As an example, you might have a set of firewall rules for workstations that are vastly different than those for servers.

Which is completely understandable.

So putting those into separate GPOs makes total sense.

---

## Application Control Policies (AppLocker)

So finally what I want to talk about is Application Control Policies or AppLocker.

Which is a feature that was introduced in Windows 7 and Server 2008 R2.

Under Computer Configuration Windows Settings, Security Settings, Application Control Policies.

This is a successor to the software restriction policies that were available since the early days of Windows.

Windows XP and even Vista.

And it provides application whitelisting and blacklisting.

Which is basically the ability to allow or deny applications from running based on different rules.

So here's a screen shot that kind of gives you an idea of how AppLocker's laid out in the GP editor.

You can create four different types of rules.

And those rule types are executable rules.

Which are, as the name implies, based on a particular executable or path that could reference the publisher of the executable, the path, or the file hash.

And file hash is just kind of a unique signature so that you can say this particular version of PowerPoint is allowed or denied.

Installer rules apply to MSI files so you can control what kind of MSI files are executed based on publisher path or file hash.

Script rules apply to pretty much any type of script file you can run.

Power shell, batch command, VBS, VBScript, or JavaScript.

And again, based on publisher path or file hash.

And then, for Windows 8x modern apps you can apply AppLocker rules to those as well, to control which modern apps can either be installed or run.

And that's you know, something like saying that the Bing travel app or MSN travel app, as I think it's called now.

Is not allowed to run on these particular machines that receive this policy.

So how does AppLocker work? So rules can either allow or deny execution.

They can be targeted at specific users or groups, and they can have exceptions.

So you can say all software from Adobe, published by Adobe is allowed to run.

Except for some particular, you know, Photoshop version that you don't want to have, being used in your environment.

You can also enforce rules actively.

So you can have Windows basically shut down execution of that application or allow it.

Or you can just audit activities.

So that AppLocker will throw off logs that says, you know, essentially, this app would have been, would have been restricted or met the rule, met a particular rule.

And therefore, would have been affected by AppLocker policy.

So the default rules, when you first create an AppLocker policy, allow kind of blanket executions.

So you can then add blacklisting rules to deny particular executions.

So blacklisting is simply, as the name implies.

Putting applications on a list to say, these applications cannot run.

And of course, it's easier to create a blacklist rule.

But it's also less secure because you don't necessarily know what you don't want to run, and that list is probably always changing.

Now, the more secure approach is to change that default rule to Deny Execution of Everything as a default state.

And then add whitelist rules to selectively allow the execution of those approved applications and processes on your systems.

Now, this is definitely more secure, but it's also a lot harder to implement.

Because you really do have to know every single process that is legitimate.

And that has a legitimate need to run on your given systems in order to create a white list rule.

So let's let's do a quick demo of AppLocker, and then we'll wrap up this module.

---

## AppLocker

So let's wrap up this module with a quick demo of the AppLocker policy.

So I've created a GPO.

Let me go ahead and edit it.

And drill into Windows Settings >Security Settings > Application Control Policies.

AppLocker.

And you'll see here my four different kinds of Executable Rules.

I also have the ability to configure kind of global behavior.

So for each rule, I can configure whether the rules are enforced.

Or whether the rules are just audited as I had mentioned earlier.

And that's a pretty straight forward thing to do and I'm going to go ahead and configure Executable Rules to be enforced.

And then I'm going to go ahead and create some executable rules.

So the first thing I'm going to do is create default rules.

And these default rules are those rules that I mentioned that essentially create a white list or an allow list for pretty much everything to execute by default.

So anything in Program Files or underneath it, anything underneath Windows, and then anything for members of the BUILTIN Administrators group anywhere, are allowed to execute.

And now what I'm going to do is create a rule that, essentially, prevents Notepad from running.

So if I go in, I have some options.

I can create a new rule manually, or I can use the Automatically Generate Rules wizard.

And that will essentially scan a folder that contains executables and create rules dynamically for each of those executables.

But we're going to go ahead and do the manual rule creation.

And I can choose to Allow or Deny this particular executable, or this particular process for a particular group.

In this case, I'm going to say Everyone.

And I can choose Publisher, Path, or File hash.

In this case, I'm going to choose a specific path.

And the path that I'm going to choose is c:\windows\system32\notepad.exe.

I didn't choose File hash because the File hash for Notepad is going to be different on different versions of Windows.

So, if I'm authoring this rule on my server 2012 R2 box and trying to apply it on a Windows 7 box, the file hash will likely not work because the file hash will have changed on Notepad between the different versions of Windows.

And that's kind of one of the downsides of file hash, is you sort of have to keep up with the different versions of each Windows or of each of each version of an application that you're creating a rule for.

So now I've got my rule.

I could add an exception based on either Publisher, Path, or File hash.

But I'm not going to go ahead and do that.

And I can enter a name.

And I'll just say Notepad denied.

And create the rule.

And now I've denied everyone the ability to run Notepad, everyone, at least, that processes this policy.

Now keep in mind, again, that this is under Computer Configuration, so this policy is being applied to computers, but it is paying attention to who the users are on those computers.

In this case, this one applies to everyone, but this one above it applies only to administrators that are logged onto that system.

So hopefully that gives you a quick idea about how App Locker works.

By and large I typically use this when, I primarily use executable rules.

There's very few scenarios where other types of rules really make sense.

Script rules would be another area that you might consider using.

But the bottom line is that executable rules is where most of the power is, in terms of whitelisting and blacklisting.

Now keep in mind, if I wanted to black list.

I'm sorry, if I wanted to create a white list, what I would do on these default rules is I would set all the Action to Deny.

So we would say, everything in Program Files is denied and everything in Windows is denied.

And then you would selectively allow other things that you want the user to do.

And again, you can imagine how problematic that might be.

But it is essentially the way that you can create a much more secure application whitelisting and application control policy.

---

## Summary

Well, we covered a lot of ground in this section, in this module, talking about, security policy, so let's recap what we talked about.

So Security policy supports a variety of security settings from password policy to firewall configuration to group membership control.

We talked about all of those and lots more beyond that.

Public key and audit and the list is pretty long.

But Security policy can tattoo a system, and in most cases will tattoo a system.

The example I gave earlier was around, security on a file system or registry.

And once that's put in place, you know, there's no easy way to just remove the policy and have the permissions on those resources get reverted back.

In fact you must explicitly undo those permissions or set them back to the way you want them explicitly in a new group policy object in order to make that happen.

Group membership is another area that we talked about.

And we talked about how you can either use Restricted Groups or Group Policy Preferences Local Users and Groups.

And that's not the only example where Preferences, GP Preferences has a sort of analogous function to something in Security policy.

Another example is in the System Services area where you can configure Windows services either the the Security Settings policy, or Group Policy Preferences services area.

So there are some capabilities that kind of overlap between the two areas.

And I think it's just important to be aware of what each of the different features are for those two policy areas.

And then Windows Firewall policy is pretty flexible.

It allows you to define both inbound and outbound rules, and you can define where those rules apply.

You've got those three profiles, the domain, private or public.

And you can set which of those are turned on for Firewall versus not.

And, you can also define IPsec policy within this area.

So again, a lot of flexibility when you create those inbound or outbound rules.

You can use predefined rules.

You can create your own custom rules.

You can do it based on TCP port or, protocol.

So, lots of options.

And then finally, we talked about AppLocker, which is this application blacklisting and whitelisting capability that's built into the box, and provides you the ability to either audit what applications are being executed, or actually block what applications are being executed.

And I talked about how blacklisting is lot easier to implement because, basically, you allow everything and then just explicitly deny those things that you know about that are bad.

But, of course, it's not as easy or as secure to do that, because you don't know the universe of bad stuff.

The alternative to that which is white listing takes a lot more work because you implicitly deny execution of everything and then specifically white list or allow those applications and processes to run.

And that does imply that you know exactly what those things are.

So that's kind of the summary of what we talked about in this module.

We're going to kind of continue on in this theme of different deployment scenarios for different policy areas in the next module, where we talk about using Group Policy to configure Internet Explorer.

---

