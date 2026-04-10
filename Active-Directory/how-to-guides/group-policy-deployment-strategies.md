# Group Policy Deployment Strategies

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Group Policy Deployment Strategies |
| Type | Automated Transcript Synthesis |

## Designing a 'Group Policy Friendly AD'

In this module, what I want to do is talk about Group Policy Deployment.

Sort of, this is more of the maybe the theoretical, or the logical around all the stuff we've been talking about so far.

Which is to say you've got the basic skills about how to manage Group Policy, how to troubleshoot Group Policy? Now, I want to talk about how to deploy it in real-world environments and I think the key here is that it's always a balancing act between Designing a Group Policy-Friendly Active Directory and meeting the needs of your AD management.

The challenge here is that AD Design is often driven by different goals than Group Policy Design.

What you have are AD being driven by the security needs of the organization, the needs to delegate administration to sub-groups of administrators, and all the applications that use AD driving you in one direction.

I'll tell, I'll give you a perfect example, there's a lot of directory enabled applications that use LDAP, and a lot of those applications are not Windows applications, they're UNIX or Linux or Java or some other kind of platform.

And often times they only use AD for authentication and they only search on a single container within AD's, you provide a base, distinguish name and given that, they benefit highly from having all of your users in a single, flat OU.

That's not a great approach from a Group Policy perspective, because then you get into the situation where, if you want to distinguish policy by a particular set of users, you then have to rely heavily on users being in particular groups, and using security group filtering.

In that scenario, you've got AD Design being at cross purpose to Group Policy Design.

Group Policy is really about ease of targeting, the ability to differentiate by platform, for example, server versus workstation or desktop versus laptop.

And then sometimes also by delegation as I showed in an earlier module, you can delegate administration of Group Policy, for example, an OU Admin.

So sometimes that driver will send you in a particular direction in terms of your AD design.

Let's look at some AD designs that are maybe GP-friendly, if you will.

This particular design starts out with a top-level OUs based on object type.

So we've got Machines, we've got People, and we've got Groups.

And then the sub-OUs are distinguished by either their role.

For example, under People, we have Interactive Users, we have Service Accounts, or their, basically their type or form factor which is under Machines, we have Laptops, Desktops, Servers, VDI and RDS systems.

And each of those have their own OU that allow you to sort of more easily distinguish and manage Group Policy based on other factors, other than just a flat OU structure.

Another approach is to take the more geographic, or business unit approach, where your top level OU's are based on either geography, east or west, or department or business unit, sales, market, etc.

and underneath each of those you have sort of this secondary structure that's identical that has users, computers and.

Under each of those users and computers, you then have, going back to our previous design or previous diagram, the sub-OUs based on role or form factor.

So, you're essentially adding a layer that is the business unit or the geographic distinction, and allowing you to sort of further differentiate and target from a Group Policy perspective.

So that's a different approach that allows you to distinguish based on business function in addition to things like Form Factor and user type.

Let's kind of poke around in, on our test system and look at how that might work?

---

## Designing a 'Group Policy Friendly AD'

Let's look at some of the designs that we just talked about and see how they might play into a Group Policy Deployment.

So I've got actually both designs that I just showed you implemented in this AD tree for the sake of example, I've got a sales OU and a marketing OU, and they both have sort of the same structure, where they have sub-OUs for Groups, Machines and Users.

Under Machines you'll see Desktops, Laptops, Servers and VDI/RDS.

And you'll notice that the VDI/RDS OU has block inherited set, as we mentioned in the Kiosk Scenario module, that this is a, a good option if you want to segregate your VDI or RDS servers or workstations, you can put them in this OU and set block inheritance and it simplifies the management of Group Policy for those devices.

Now, again, I've got different form factors and I could have underneath the Marketing OU and the Machine's OU I can have a GPO that sets policy for all Machines, sort of any common policy that may be applied across all computer objects can be set at this Machine's level.

Now the only downside to this kind of business unit approach is that, let's say I have some GPO settings that apply to all machines across the organization.

Well, that GPO that I define has to be linked both here at this machine's level and also down here under the Sales OU structure at the machine's level.

The alternative, of course, is the first design I talked about which is more of a object type based, and that's represented by this Machines and People top-level OU's.

And the machines top-level OU has a similar structure to the marketing and sales OUs in that each of the different types of machines is broken out by form factor, by function.

And then I can apply if all machine accounts in the domain are under this structure, I can very easily apply a GPO or link a GPO to this level that has Computer Policy that applies to all systems regardless of form factor.

Then I could come down here at the desk top level, apply policy to desktop based machines that's different from laptop machines and then for sure Servers are going to have a different set of policy, and so I can apply that at this level specific to servers.

And similarly, with the VDI and RDS systems.

So, this allows me a little bit more flexibility in terms of targeting GPOs.

I don't have to do as much linking.

And I could also have further differentiations underneath for example under the Servers OU I can have App Servers and I can have Web Servers.

And so I could be applying policy at these sub levels as well.

Now on the People side with this structure it's a little bit more challenging if all the user accounts in the domain are under the People OU.

I probably will have the need and desire to be able to sub-divided further underneath People, so for example, interactive users here in service accounts, or I might even go so far as to have different business units, people accounts under here, so I might have, for example.

A Sales OU for the sales users.

And a Marketing OU for the marketing users.

So I have some flexibility here.

This still allows me to set again, policy that applies to all users linked here at this People OU.

And then policy that is specific to Marketing and Sales down below here.

So both of those approaches are completely reasonable.

And depending on your business needs will drive sort of which direction you go with this.

I tend to like just in terms of my own personal preference, I tend to like this Machines and People at the top level approach.

Because I like the idea of being able to link in a single place to apply sort of global policy and then be able to subdivide and segregate other policy based on sub-OUs.

Now, the one thing you don't want to do is get too deep in your OU structure.

It becomes a lot more difficult to manage and you really just don't want to have, you know, five or six levels deep of subdivisions.

You really only want to subdivide sort of to meet the needs of Group Policy and your AD administration, so balance those two.

And if you find yourself having to create too many sub-OUs to, for example, segregate group policy, you might then start looking at filtering.

So doing security group filtering or, you know, for example, WMI filtering.

And a good example of that might be, so within the Sales OU under People, you might have sales managers and sales users.

We don't really want to have to create an OU called Users and an OU called Managers.

Because people are going to move around, shift around and you don't want to have to be moving people around OUs in order to accommodate group policy.

So that's a scenario where you can use security groups and probably get the job done more efficiently.

I would say that as a rule of thumb, if you have to move objects between OUs to meet your group policy targeting goals.

Then you're probably taking the wrong approach and should rethink using, for example, Security Group Filtering or WMI Filtering to accomplish those goals.

So that's just kind of a high level overview.

There's obviously lots of different design approaches you can take with Active Directory.

I've seen people take design approaches where under the Machines OU, they'll have different OS version OUs.

So for example, Windows 7 might be underneath the Machines OU, instead of just this form factor type.

It tend to avoid OS version approaches, because OS versions change as machines get upgraded.

And again, if you're having to move objects between OUs to accommodate your Group Policy or design goals, then maybe you need to rethink it.

I like the notion of using WMI Filters for object version or for, I'm sorry, for OS version testing.

Very tried and true method.

So having a WMI Filter that applies to all Windows 7 machines linked to the GPOs if you want to target in the desktop, so you only the Window 7 machines you can use a WMIC Filter for that and it's a lot more efficient.

And certainly, a lot easier to maintain than creating new OS version OUs.

So again, lots of approaches, but there are some that have sort of shove, shaken out over time that have proven to be more sort of efficient than others.

---

## Best Practices for GP Deployment

Now that we've sort of gone over some of the different approaches in terms of designing your AD, I want to talk about some general best practices around Group Policy deployment.

So, I have a set of rules that I've sort of established over the years that are sort of, are tried and true in terms of how you should approach Group Policy deployment.

Rule number 1 is, link your GPOs as close to the intended targets as possible.

So this is this notion that we don't really want to have to do security group filtering if we can link GPOs really close to their targets, whether they be computers or users.

So instead of linking at the domain level, all of your GPOs and putting a bunch of security group filters on them to control, which computers and users process them.

We want a link down in those OUs that we talked about in the previous example.

So at the, for example, if we have a user policy that applies to sales users, we want to link it at the sales user OU in order to get the closest possible targeting.

And sort of the corollary to that is using filtering, security group filtering, WMI Filtering or Group Policy Preferences item level target should be done on an exception basis.

Again, organizing AD to balance AD administration and Group Policy needs is a key piece here and I talked a lot about that in the last section.

Limiting the fingers in the pie.

What do I mean by this? What I mean is keep, the fewest possible number of administrators managing Group Policy as possible.

This prevents people from stepping on each other from not following standards and from just generally making changes that other people don't know about.

So, having control over who's editing Group Policy.

Remember, our discussion about Group Policy delegation.

It's very easy to control who can do what within Group Policy using the delegation model and I highly recommend that.

So avoid that one setting, one GPO.

So this is a common thing that folks will get into, where they need a setting and they decided to create a new GPO for that setting.

And it's fine, if you're doing it a couple of times.

But if it becomes a practice where you have 100 GPOs each with one setting in them, then you probably need to rethink that.

And I like to group settings by function or type and especially as delegation requires.

So for example, I might put all of my security settings in a stand alone GPO, so that they can be delegated to the security administrator in my organization.

Avoid that copy and paste phenomenon that I talked about in an earlier module, so you need some settings that you have in another GPO.

Well, the handiest thing to do is to just copy and paste that existing GPO.

Problem is that oftentimes, people won't go in and clean up the settings that they don't need from that source GPO and so then you've got lots of redundant settings running around.

So, I try to avoid.

If you're going to do copy and paste, make sure you're going to go in and clean up the settings that you don't need out of that new GPO.

Group Policy settings performance.

Before Windows 8.1, these three policy areas Group Policy, Drive Mapping, GP Preferences, Drive Mapping, Folder Redirection and Software Installation require a synchronize foreground refresh and they should be separate from other policy areas in GPOs.

So keep these separate from let's say, Admin Templates or Security and you'll improve your performance.

And I'll talk a little bit more about this in a second.

Avoid that, always wait for the network at computer startup and user logon policy that I had mentioned earlier that sets every policy processing foreground cycle to synchronous.

If you don't really need this, then don't set it universally.

I know a lot of shops that set this universally and they're automatically inflicting their users with a penalty at boot-up and start and logon time.

And then finally, always back up your GPOs.

We talked about GPO backups and how easy they were to do.

Always, always, always do this.

A lot of people don't get into the habit of this and get bitten by it when they make changes or have to roll back GPO changes.

I want to talk now about kind of of the design approach with the actual GPO and how you group settings in GPOs.

A bunch of years ago, I came up with this.

These definitions, monolithic and functional GPOs, and I've been using them ever since as a way of classifying the two different types of GPOs.

Where monolithic are GPOs that create, that contain settings from a lot of different policy areas in a single GPO.

So you, group software installation settings, admin template, whatever, in a single GPO.

And that's contrasted with functional GPOs, which are really where you have one or more settings from a single policy area.

The example I just gave was security policy.

All security policy, in its own GPO, and just security policy in that GPO.

And that would be an example of a functional GPO.

So, which do you deploy? Well, most environments are going to have a combination of both, which is perfectly reasonable.

It ends up being driven by factors like the delegation needs, the complexity.

You know, certainly monolithic GPOs tend to be more complex, because you've got a lot more stuff going on in them, but they're great for delegating to a OU administrator.

You make one GPO, with all the settings that that OU administrator needs and say, you have rights over this GPO and no others.

Also security mandates might drive this.

Do I need to delegate my security settings to the infosec team? If so, then I might want to just have a separate GPO for just those settings.

And then, you know, each type is going to make sense in a certain situation.

There are some considerations around GP processing performance that might lead you to one versus the other and I, I give you the hint that it's the functional type that tends to perform better.

Because of the reason I mentioned in my previous list where if you have synchronous policy settings like Folder Redirection, Software Installation or GP Preferences Drive Mappings, in with let's say Admin templates and a single GPO.

Whenever you make a change to any of those areas, it will automatically trigger a synchronous foreground refresh.

So having those synchronous areas in separate GPOs can be better for performance.

So a little bit more about functional GPOs, really used to isolate the single setting or the group of settings, and, you know.

For example, account policy.

Perfect example of a functional GPO.

A lot of people use the default, the main policy just to do password policy for the domain.

And that's a good approach.

But you can go overboard here.

Again, you don't want hundreds of GPOs with a single setting in them.

Think about grouping settings based on function or type.

If you're going to use the functional approach, have, let's say, all admin template settings for the marketing folks in a single GPO.

Monolithic GPOs, ideal for delegating to administrators.

You know OU administrators, because you can create, like I said, a single GPO with all the settings they need, delegate edit rights to that GPO.

And they're done.

And it has a tendency for those scenarios to keep those settings in a single manageable delegated place.

And it, for them, for those OU administrators it kind of eases troubleshooting because it gives them the ability to look in one place to find and fix potential problems, and that's a good thing.

---

## Best Practices for GP Deployment

Okay, just to underscore what I just talked about, with respect to functional and monolithic GPOs, let's kind of look at two GPOs that I've created to illustrate each.

So I've created a functional GPO called Functional GPO.

And you can see that it only contains per computer security settings in it.

So this is an example of a GPO that can be easily delegated to a security group.

Using delegation I can control who can edit this GPO and it only contains a single policy area.

Now by contrast, I've created this GPO called Monolithic GPO and if you'll recall from my definition, Monolithic GPOs mean GPOs that contain settings from lots of different areas in a single GPO.

So for example, I've got a log on script to find, these are all per user settings in this case.

I've got some admin template settings to find, I've got a GP preferences drive map to find, and a registry set, setting to find under GP preferences.

So, a bunch of different, in this case four different policy areas are defined in this GPO all for the user.

And again I could very easily use this to, for example, if I wanted to delegate administration of a single GPO to the sales admins.

I could link this GPO to the sales OU under the monolithic GPO here.

Just link it to the sales OU, and give delegation rights to the sales admins group to be the only group that can edit this GPO and give them edit rights but don't give them the ability to change the permissions on the GPO.

And so sales admins could then come in and edit this GPO to their heart's content, they wouldn't be able to create any new GPOs at the sales OU or link any new GPOs at the sales OU level.

But they would be able to edit this single monolithic GPO where all of their settings reside, and so that's the whole purpose or the value of monolithic vs the functional GPO.

Now, remember, I mentioned that from a performance perspective, you probably don't want to group one of those synchronous policy areas, like GP preferences drive mappings, in with other policy areas.

In this case, wh, wa, was exactly what I've done.

It wouldn't be an issue on Windows 8.1 because drive mappings has been changed to run asynchronously in Windows 8.1.

But in Windows 7, if, if clients were processing this GPO.

What you'd end up have happening is, every time you made a change to any setting area within this GPO, whether it be scripts, administrative templates, or registry, you would end up having the drive mapping extension trigger a synchronous foreground refresh for the next logon.

So the user would end up waiting for that synchronous foreground refresh at next logon just because this Drive Maps setting is grouped in with all these other settings.

So again, it's a good idea to avoid doing that if you're in a Windows 7 world and you're trying to keep performance at a maximum and trying to minimize the impact of Group Policy processing on user experience and logon.

---

## Designing for Performance

Now I want to finish up this module by talking about performance and group policy performance in particular.

Remember that I showed in the group policy results troubleshooting section, that you could see how long group policy processing was taking for a given computer or user.

Or actually you could see the time elapsed for each client side extension, so you could see very specifically what time group policy was spending doing its thing for computers and users.

And I think it's important to note that sometimes group policy gets blamed for a lot of performance issues when it's maybe not the cause of them but there are some things a group policy legitimately can impact and those are the things that I want to talk about.

And there are some things you can control and some things you can't control.

Some things that the OS does and some things that you do with your design.

So I want to talk about those design-based things first.

The very first thing is the use of scripts.

And again, I've, I talked about this in the script's module but.

Group Policy-based Scripts can be problematic.

They can take a long time to run, if you don't have good testing and logic in them.

They can potentially hang and cause up to ten minutes of delay if they're hanging, which is not obviously great for the user experience.

And they're just sort of problematic, because most folks don't implement them with sufficient instrumentation and logic to prevent them from spending a lot of time doing work they don't need to do.

So, if you can avoid using scripts in favor of things like GP Preferences, I always recommend that.

The second thing are using expensive what I call expensive Client-Side Extensions.

And the two examples that I like to use are File and Registry Security.

These are under the computer configuration Windows Setting Security Settings.

And this is where you can set File System and Registry Security.

And again, this can run every 16 hours by default, because it's part of the security extension.

It can really churn through disk, I mentioned this earlier.

I tend to do avoid doing File and Registry Security setting in Group Policy.

Use a script that runs once and gets done with it or runs once every once in a while.

Perfectly reasonable to take that approach.

I don't think it's worth doing it in Group Policy.

Another one is you end up having too many GPOs being processed for a given user or computer.

This can happen when you have way too many functional GPOs with just a couple settings in them and the overhead of having to evaluate all those GPOs, figuring out which apply and then processing them can become a significant burden on Group Policy Processing.

So always think about, you know, as you're creating new GPOs or as you're linking GPOs to existing AD hierarchies, how many is too many? And I would say that if you, a given user or computer is processing more than ten or twenty GPOs, you have to start and think, well maybe I can combine and consolidate some of these.

The grouping of Client-Side Extensions.

This kind of harkens back to what I just talked about with synchronous and asynchronous extensions.

If you can keep the synchronous extensions, Folder Redirection, Software Installation and Group Policy Preferences, drive mappings out of the GPOs that have other settings in them so on their own.

Then you can minimize the impact when you make changes to those GPOs of having those synchronous extension force a foreground refresh.

And then finally, Expensive WMI Filters.

What do I mean by expensive? Well, essentially some WMI Filters can take a long time to evaluate.

I wrote a blog post a couple years ago about a WMI class called WIN32_Product, which was ostensibly designed to basically query all of the installed applications on a system.

But it had some really nasty side effects that caused it to really turn through lots of CPU and memory when it was running and it would take literally minutes to run.

So this is not a WMI query that you're going to want to use and there are others that can take a while based on that.

There are WMI queries you can do to determine if a particular file is on the file system.

And, you know, that may not be the smartest thing to use WMI for.

I have a little tool if you go to GPOGuy.com, which is my website.

There's a free tool called the WMI Filter Validator and one of the things that it does is it validates to see if a filter will evaluate to true or false on a given system.

But it will also times the query to see how long it takes and you can use that to sort of, as a thumbnail to sort of guide you on whether a query is too expensive.

Let's then talk about OS Features that have an impact on Group Policy performance.

Microsoft introduced this concept of Group Policy Caching in Windows 8.1.

It sounds really good, but it actually doesn't do much.

It only runs when one of the synchronous CSE's requires a synchronous foreground reflash, like Folder Redirection or Software Installation are the only two left on Windows 8.1 that require a synchronous foreground refresh.

They fixed drive mapping, so it no longer requires that.

So the Group Policy Caching only kicks in when one of those two extensions is required a synchronous foreground refresh.

So if you're using a lot of Folder Redirection in Software Installation, this may benefit you.

But otherwise, you probably will never see the effect of this.

Another thing that they added in Windows 8.0 is called Fast Startup.

And this is a pretty much unrelated to GP Feature that has a GP impact.

So, it allows Windows to start from a shut down more quickly.

It's essentially, hibernating the OS when you turn off a machine normally and it's enabled by default.

It's in effect whenever the user selects shut down.

But Group Policy gets bypassed when the machine comes backup, because fast startup essentially bypasses the per-computer boot up processing.

So if you have startup scripts, they don't run.

If you have security settings, they don't run.

Whatever, per-computer settings you have don't run.

So while it's a great benefit for the user, Group Policy gets a little confused and it does require the user to do an actual restart in order for that per computer policy to run, so they have to do a restart rather than a shut down.

Now you can turn this off using this admin template setting that I list here, and I, I, recommend that if your, in a critical environment where you need that computer settings, those computer settings to be refreshing in the foreground, then this is the policy that you're going to want to enable.

And then finally, Microsoft added this so-called Logon Script Delay in Windows 8.1 to help reduce the contention that happens at user logon time when there's a bunch of stuff going on, including logon scripts.

So what happens is they actually delay the execution of logon scripts by up to five minutes, that's the default.

So the user logs on and then five minutes later, their logon scripts run.

Well, this can really mess you up if you're relying on logon scripts for drive mapping, but it does reduce that contention at logon time.

You can actually turn this off, there's a policy under computer configuration policies admin template system group policy that lets you turn off this delay in Windows 8.1 and Server 2012 R2.

But essentially, it's there and on by default, so it's good to know about that.

---

## Summary

So let's summarize what we've learned in this module on Group Policy Deployment.

So again, the best AD design balances AD and GP management needs.

So being able to do things, like delegating administration and efficiently targeting GPOs is kind of what I mean by that balance and you can take a number of different approaches to that.

I showed two approaches.

One was a Role-based approach and the other was a Geo or Business Unit-based approach that allows for OU structures that kind of optimize for both AD and Group Policy management.

And I set aside a number of principles that I talked about for good Group Policy deployment design.

And one of those, probably the first one was deploying GPOs by linking them as close to their intended targets as possible.

And only using Security group or WMI filtering or item-level target as an exception.

I think this is a really good tenant to follow.

Sometimes, you won't be able to, based on your requirements.

But if you start from this premise, then you'll have as efficient a Group Policy design as possible.

I also introduce the concept of Functional versus Monolithic GPOs.

Functional being GPOs that have a single policy area implemented in them.

Monolithic, meaning multiple policy areas implemented in the GPO.

And I also mentioned that both can have their place in the environment.

You might use Monolithic to delegate administration to a single OU administrator, whereas Functional is good, for example, Security settings.

Functional GPOs can perform better.

Primarily, because of that issue I mentioned around synchronous and asynchronous extensions.

And while we're on that topic, you can improve performance by reducing the use of Group Policy-based scripts.

Not mixing Sync and Async Client-Side Extensions and keeping an eye on the number of GPOs that get processed by users at computers.

So all of those methods can have an impact on GP Processing performance.

And therefore, the user's experience.

And I mentioned also WMI Filters that they, some filter queries can be expensive and can cause slowdowns in your environment, so you want to keep an eye on that.

And then finally, there are some OS features that have been introduced in newer versions of Windows, like GP Caching and Logon Script Delays.

These are designed to reduce the impact of GP Processing, but they can also change the behavior of Group Policy.

So, it's important to keep those in mind.

And in the case of GP Caching you are, based on the way it works, you are only likely to see it.

You will only see it, if a synchronous foreground refresh is happening.

Only is caused by Folder Redirection and Software Installation in Windows 8.1.

So not very often, unless you are using a lot of those extensions.

And the Logon Script Delay feature introduced a five minute logon script delay.

So that reduces contention at user logon time but also delays the execution of logon scripts.

Which is yet, another reason in my estimation why using something other than logon scripts for important stuff, like drive mappings and printer mappings is probably a good way to go.

So in the final module of this course, I'm going to spend some time talking to you about scripting Group Policy management.

We're going to start off by talking about and primarily talk about PowerShell, but also introduce a little bit about VBScript within the module.

---

