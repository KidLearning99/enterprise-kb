# Policy Scenarios - Deploying Software Using Group Policy

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Deploying Software Using Group Policy |
| Type | Automated Transcript Synthesis |

## Capabilities and Features in Group Policy Software Installation

Okay in this module I'm going to talk about deploying software using group policy.

So one of the capabilities of Group Policy is the ability to deploy or install software.

It's sort of a, I would call it a rudimentary software distribution capability.

So unlike products like System Centered Configuration Manager or other third party software distribution products, the Group Policy software installation feature in the group policy engine is fairly limited in what it can do.

So what it does provide, other than that basic software distribution capability, is you're going to only install MSI or Windows Installer packages.

So that the setup has to be in an MSI format in order for it to work with The Group Policy software installation feature.

There's a small fringe exception to that, that I'm really not going to cover because it kind of limits the capabilities of the GPSI function, but, and, it's called Zap Packaging and it's, it goes way back to the very beginning of Group Policy, and really isn't used anymore.

But it does provide a limited amount of capabilities around non-MSI deployment.

You can do either per-computer or per-user targeting, as is normal with Group Policy.

So you can deploy a package to a computer or group of computers.

Or you can deploy it to users.

So, whenever a user logs on to a computer if they have a package deployed to them and it's not already installed it gets installed on that computer.

You can do some level of life cycle support with the group policy software deployment feature so that includes things like upgrades, patches and removals.

You can also redeploy software so if you, if it's been deployed and for whatever reason it's not, getting there or it's not, triggering a deployment on the target system, you can actually do a redeploy.

Now, let's look at what it doesn't provide, which is probably more relevant.

I will say that, to put it in context, a Group Policy software deployment feature is really designed for small or medium shops that don't want to spend the money of a big, you know, software distribution package like System Center Configuration Manager, and just need some basic software deployment capability.

So what software deployment and Group Policy does not provide is, as I've mentioned, support for, you know full featured support for other types of setup packages, other than MSI.

So .exe, or whatever deployment packages you might have you know, a good example might be the new modern app format within Windows 8 and above.

No support for any of those kinds of packages.

No inventory or reporting on the distribution of software.

So, you, there's no way, centrally, to find out if, you know? If you've deployed to 100 computers in an OU, there's no way to know that all 100 of those computers have gotten the package.

There's no reporting back.

There's, you know, the upgrade feature is not what I would call easy in Group Policy software deployment.

It's quite brittle and somewhat limited, so there's really not what I would say robust upgrade capability within the product.

And there's no concept of, kind of, per node management of the deployed software.

So you can't go in and, you know, re-push a package to just one system.

It's limited by virtue of the fact that with group policy targeting, you're typically generally targeting a whole OU, or a group of computers or users.

And so it's not, there's no real concept of being able to manage a single nodes softer deployment state.

Redeploy to just that node out of the 100 that have been targeted by that GPO.

So really kind of limited in that respect.

Now let's look at some of the features that are in Group Policy Software Installation.

So, there's this notion of per user assignment or publishing, and I'll talk more about assigning and publishing and the differences of those in a bit.

There's the support for upgrade relationships, and this is how they're referred to in GPSI land.

It's basically this ability to say that if you'd already deployed version one in GPOA, you can create an upgrade relationship to version two of the same package in GPOB.

And have all the clients that have installed you know, the version one get the upgrade.

There's support for something called transforms which is a feature of Windows Installer MSI packages that allow you to modify a vendor's MSI setup to do custom installation.

So as, a typical example might be, in the old days you could deploy Microsoft Office when it shipped as an MSI.

And if you only wanted to install Power Point, you could write a transform for the Office setup that would only install Power Point.

Per-computer assignment is another feature.

This is the ability to assign applications to the computers.

You also get automatic elevation of privileges during installation, so if the user, if the application requires admin rights and the user doesn't have it, then GPSI takes care of that.

And then this kind of ability to do what's called install on first use, which is that the application is not really fully installed on the target, whether it's a computer or user, until the user triggers something that requires it.

For example, they click on a Word document in an email attachment and Word gets installed on demand.

So to define software installation policy, you can find it under Computer Configuration or User Configuration in these paths.

Per computer packages can only be assigned, so you can only do assignment of a per computer package.

And it does require a reboot of the computer to trigger the install.

Per user packages can be published or assigned.

Published is for kind of install on first use or install by the user types of packages, where you don't necessarily need the software right there right now.

Assigned packages do require a re-log on by the user to get the install.

So let's take a look at this and, like, dig in on a real system and see what this actually looks like.

---

## Capabilities & Features in Group Policy Installation

Okay.

Now I'm on my server 2012 R2 system, where I have my active directory installed and my Group Policy objects.

And I'm just going to kind of spin through the kind of basics around software deployment, software installation.

I've created a new GPO called software installation policy.

So I'm going to go ahead and bring up the GP editor on this and just kind of show you how to get to the software installation stuff.

So if you're going to be deploying per computer then you'll go to this Software Installation node, right click and choose New Package.

And you'll be able to deploy a per computer assigned package.

If I go under User Configuration, similar scenario.

I can go to Software Settings and Software Installation under User Configuration.

New Package and I'll have the ability to choose a package that I want to install and have options on installing it which I'll show you in a little bit.

One thing I do want to mention in this demo is that there's some capabilities around the defaults that you see when you're deploying a new package that you can set from within this UI.

And it's not all together obvious.

A lot of people often miss it.

If I right click this software installation node and say Properties.

What you'll see, if I get my properties dialog back, is you can set a default package location, in other words if you have a server share out there where you want all your packages to reside, you can set that and it will automatically browse to that when you're publishing or assigning a new package.

You can also ask it to prompt you for which of these you want to by default publish, assign, or advanced.

And in advanced it just gives you the ability to set all of the options.

Instead of automatically just publishing or assigning without any additional options, you can go ahead and click advanced and you'll get into the advanced option.

And you can set the user interface options, and I'll talk a lot about what this means in a little bit.

But this essentially controls how verbose the installation appears to the user.

If I click onto the Advanced area here, you'll see I have a number of advanced options that I can set, including the uninstall applications when they fall out of scope.

Including OLE information, this is really old stuff related to COM systems, or COM applications and fairly rarely used these days.

And then behavior of 32-bit applications on 64-bit platforms.

So, that, those are options.

Again, what you're doing here is you're just setting the defaults for these values when you publish a new or assign a new package.

File installations.

This one's kind of interesting, remember I mentioned install on first use? Well, what I can do, let's say hypothetically I have two applications that handle PDF files.

If PDF is one of the extensions that's available to me, and I have two applications already published in GPO, I can control which application will launch a PDF file.

Preferentially using this UI, so if the user gets, for example, two PDF readers assigned to them, you can set which one launches using this dialogue.

So very much a corner case, but very much supported in software installation.

And then finally, the Categories option lets me define categories of applications so that I can group applications when I publish them.

So, this is only for per user publishing, where the application appears in the Add/Remove programs control panel applet.

And I'll show you that in a little bit.

But essentially, this is the way you can define these categories of application types.

And I've defined one here called utilities.

I could also define one called productivity, and that, that might be where I put office applications, for example.

And those categories will, can be assigned to the package when the package is deployed.

So, that's kind of a quick run through of just getting into and finding your way around the software installation.

Next, we'll talk about kind of the details of how you deploy a package.

And I'll dive in and give you a demo of that in a bit.

---

## GPSI Best Practices

So now I want to show you some of the options that are available, and some of the best practices around deploying software packages using group policy.

When you deploy a package, you'll see this advanced page that comes up.

And it gives you the ability to set a bunch of options on the package deployment.

So for example, you can set the deployment type to either published or assigned.

And if this were a per computer deployment, you wouldn't see the published option or it would be grayed out, so you'd only have the ability to assign.

And then you've got a bunch of deployment options like the install-on-first-use option which allows you to install an application by the file extension.

And what that means is as in the example I gave previously, if you get a PDF file as an attachment and you don't have a PDF reader installed, the first time you click on that PDF if the PDF reader has been published to the user, it will automatically trigger the install.

The install will occur.

Package the reader will be installed and it will open with that document that the user clicked on.

So, this is kind of the install-on-first-use model.

The other option below it, uninstall, is Its application when it falls out of the scope of management, means that the app gets removed when the GPO is no longer in scope.

So for example, if the GPO that delivered the app, the Adobe Reader package gets deleted or unlinked, or the user moves from one OU where that GPO exists or is linked to another where it's not, then the package will be deinstalled when the user logs on the next time.

And then, you have the option for published apps to not have them appear in the add remove programs control panel applet.

That is the default, so when you publish an application, it will automatically appear in add remove programs.

But, you can tell it I don't want to do that, I'd rather, you know, basically not have it appear.

So the user doesn't have a choice of when they install, it gets installed for example, when they click on that PDF file.

And then, you can also choose the UI that appears when the application or the package is installing.

So, in most cases, you're going to want to choose Basic, unless you want the user to get a lot of feedback about what's going on in the installation.

So Basic is as close to silent as you can get, in an installation, of group, using group policy for a package.

Now, some other package options, there's a bunch of tabs within this dialogue.

You can set upgrade relationships, and I talked about this earlier.

So, if I had packages that upgraded my current package, I could add them here.

Again, I can use, I can select the categories for the application that I'm publishing.

So as in the example I gave previously.

If I have a utilities category, I can assign it to this Adobe Reader package.

And I can set modifications.

This is where you can use MSI transforms to modify the default behavior of the install package.

So I can add an .MST.

As in Tom file to this package and that transform will modify the package and the package's behavior as it's installed by the computer or user.

And then finally, I can actually set the security on the package.

And this is kind of a, I'd call a poor man's security group filtering for individual packages.

It lets you if you remove, for example, the read permission from a particular group for this package, then even though they might process the GPO that contains this package, they will not be able to install the application because they don't have read permissions to it.

So you could have, for example, a GPO with five packages deployed to the marketing user's group.

But if there's a particular user in the marketing user's group that does not have read access, perhaps you deny read to the Adobe reader package,.

They would not actually install that package.

So there's some granularity you get with the Security tab in terms of who actually runs those package installations.

So let's shift gears and talk a little bit about best practices.

The one best practice that I always, always, always recommend is to store your MSI installer packages on distributed file system or DFS shares.

And the reason that you need to do this is, if you stick it on a single server and a server share, if that server needs to go away, you retire the hardware or the server crashes and your unable to recover it that.

That link between the server UNC path, the share path, and the software that's been deployed to all those computers or users, is broken.

That link is broken and essentially the package is orphaned, you'd not be able to.

It won't de-install from those systems or those users, but you wouldn't be able to upgrade that package anymore.

You wouldn't be able to patch that package.

So I highly recommend, for a number of reasons, using DFS for storing your installer packages.

And this is just a kind of a screenshot of the DFS Management tool and how you can create a namespace.

In this case, I called it Packages.

And underneath it, there's a link to the installer's share.

Use per computer assignment, unless you want the application to follow the user to every machine.

So, you know, there's probably very few situations where you want to use per user deployment of applications.

Because, it does mean that every time the user logs onto a machine where that application isn't installed, they're going to get that application installed.

And if it's an application that has licensing restrictions, you're going to leave machines littered with installed applications that may exceed your license count.

So, I typically recommend, while you can do per user, I typically recommend to stick with the per user, I'm sorry, per computer assignment.

And then use the administrative install option of the MSI package, and this allows the package to be patched.

And I'll talk about this in my demo, but essentially what this means is when you, instead of just copying the MSI file to your DFS share, you're going to actually do an administrative install of the package and that does a special kind of deployment of the package to that share that allows it to be patched down the line.

Don't allow users to de-install GPSI deployed packages, the reason for this is, that essentially orphans that machine.

You can't get the package back to the machine using group policy if the user has sort of broken the relationship between group policy and the machine.

And this is kind of another brittleness of the GPSI feature that you definitely don't want the user being the one that de-installs a group policy deployed package.

And then finally, transforms can only be applied at the time that you deploy the package.

So if, for example, you deploy an MSI and then you decide afterwords you want to modify it with a transform, you can't do it.

You can't add that transform after the package has been deployed.

You'd have to recreate the package, redeploy it to all your clients with the transform assigned to it.

So let's go now and look at walking through kind of a sample deployment using group policy software installation to see how this works, and how you can leverage it in your environment.

---

## Deploying Software Using Group Policy

Okay, now what I want to do is take you kind of start to finish through a package deployment.

So, what we've got here is a DFS share, so this is a domain based DFS share.

You see my domain name, packages, installers, and then a sub directory called Adobe Reader.

Now what I'm going to do, I have the Adobe Acrobat msi.

And, I'm going to go ahead and do an administrative install.

Remember, I talked about that.

Of the acroread.msi.

So, I've got this command, msiexec, which is the msi or Windows installer executable, /a.

With the package path to the actual msi.

Now if I execute that it looks like it's going to install Acrobat but what you're going to notice is it says here it's going to create a server image of Adobe Acrobat Reader at the specified network location.

So I'm going to go ahead and click Next and then I'm going to give it this network location of my DFS share.

And, I'm going to have the administrative install occur into this Share, and you'll notice that it's copying a bunch of files in the background over to that Share and getting Adobe all setup to go.

And it doesn't take very long to do that, it's going to get that setup there in just a few seconds.

And when it's all complete, I now have an administrative install on this DFS share.

And remember, I did that because I want to be able to patch Adobe Reader with subsequent patches as they come up.

So I'm going to go ahead and now go back to my GPO.

Right click under User configuration.

Software installation > New > Package.

And what I'm going to do, I'm not going to point to the local C drive where this package actually resides.

I'm going to point to the DFS share because what I want, this is the path that the clients are going to see when they install this package, so they need to be able to see the DFS share.

So, I'm going to go ahead and dig into that Adobe Reader and select the MSI file.

And now, I get the options that you saw in my previous discussion.

I can choose Deployment.

I'm going to say Published, Autoinstall.

I'll say Uninstall when the GPO falls out of scope.

I'll leave the user interface as maximum, because I don't mind them doing a per user install.

No upgrades.

For categories I am going to choose the Utilities category, and I don't have any transforms, and I'm just going to leave the security at the default.

So now I've got my package that's deployed.

Now I'm going to go back to GPMC, and I'm going to link this to my sales users OU.

So I've got my software deployment, software installation policy now linked to my users' OU.

Now what I'm going to do is come over to my Windows 7 client.

And, the first thing I want to do is actually do a GP update force.

Now I'm logged in as a user who's in that sales user OU.

The reason I want to do a GP update force is software installation, if you remember from a previous module, is one of those client side extensions that requires a foreground synchronous refresh in order to take effect.

Now, by default, if I were just to log off and log back in, that wouldn't be sufficient to trigger that foreground synchronous refresh.

I'd have to log in a second time.

What this GP update force command is doing for me, is it's going out and getting the fact that I have that new software deployment policy assigned to me or published to me and it's setting a flag on this system that tells it that the next log on should be synchronous.

That will allow me to get that package on the next logon.

And you'll notice here that it's detected that certain user policies are enabled that can only run during logon.

That would be my software deployment.

So, I'm going to go ahead and log off.

And now I'm logged back into my client system and I should've now gotten the application package that I had just deployed.

So let me go to Add/Remove Programs.

Remember, this is a published application.

And if I go to this option called Install a Program from the Network, there you see my Adobe Reader package that's been deployed.

So now I have this available to me as a user if I wanted to install it from here.

So now what I'm going to do now that I have this in Add/Remove programs is I'm going to go ahead and click Install.

And it's going to go ahead and get that package.

Give me the options I can install updates automatically, or, if this had been a basic UI, I might not have seen all these options, it might have just run through the installation without giving me any choices.

But I chose to make it a maximum UI, so I'm getting all these options.

And I'm going to see that once I get Adobe Reader installed, I'm now managed, I'm a managed system as far as that Group Policy deployment is concerned.

So as soon as this completes, what I will be able to do is click on, you know, any sort of PDF document that I have or any other file that is launched by this particular application and I'll be able to get access to the app.

And that's all delivered via Group Policy.

So it's been, it's given me the capability of getting an application down to the user or in this case, published to the user in a fairly quick manner.

And the app is now there and available for me to use.

So if I go in under Documents and I double-click this, then that association has happened between the new app that I've just installed and my PDF file and I can go ahead and open it.

---

## Managing the Software Lifecycle

Okay.

Now, I want to get into how you can use Group Policy software deployment to manage a software lifecycle.

What I mean by lifecycle is sort of that birth updating and death of a package as it's deployed to a client.

So, some of the things you can do within the GPSI lifecycle capabilities are patching of exist packages.

Upgrades of existing packages, so I could go from Adobe Reader 11 to Adobe Reader 12 or Office 2007 to 2010.

Actually, Office is a really bad example.

Because as of about Office 2007, Microsoft actually broke the ability to use Group Policy software installation to deploy Office.

So you aren't going to really be able to do that to deploy Microsoft Office using Group Policy software installation for a variety of reasons not the least of which is that it's no longer packaged as an MSI file.

So there's a lot of challenges around using it with group policy.

But in any case, you can do patching, upgrades and redeployment of an existing application.

So, if for whatever reason, you need to redeploy and I'll talk about where that comes into play for some of this lifecycle stuff, you can do that.

And then the removal of the package, if you no longer need the package installed or you want to remove it form certain systems.

Then that option that I showed you earlier about having a GPO remove the package when the scope of the GPO is no longer targeting that user or computer.

So, all of these things are possible with Group Policy software installation to a certain degree.

Each of those operations, of course is done against the entire deployed base, as I kind of alluded to earlier in the limitations around Group Policy software installation.

There's no such thing as being able to phase in patches.

If you've deployed a package in a GPO to 100 computers, then when you update that package, when you patch that package, the phasing in is really governed by how quickly the computer is rebooted or the user re-logs in and that's really the only governing factor you have, the only sort of limiter.

There's no way to say, I only wanted to go to these ten systems.

So once the package is deployed, you're kind of stuck with whatever target base you've deployed it to.

Now you can certainly target the same application to smaller subsets of machines or users and then use that as your sort of deployment strategy going forward, but you have to consciously do that by using, you know, features that I've talked about before, such as security group filtering or WMI filtering to target those applications to particular user or computer populations.

So again, all of these lifecycle options, if you want to call them that, require that foreground refresh to take effect.

So you either need to reboot the computer to get the new upgrade or the new patch or you need to re-logon and all of those things need to happen.

So let's spend a little bit of time talking about how we redeploy or remove a package.

So once the package is out there, all you need to do is right-click on package and choose All Tasks.

And you'll notice that you can do three different things for a published application in this example.

You can actually change its deployment mode from Publish to Assign, you can remove it.

And then when you remove the package, then the behavior at the client is governed by how that checkbox was set, that uninstall when package falls out of scope checkbox, how that's set when you deployed the package.

So, if that's set to, you know, basically checked, then when you click Remove, then the software package will be removed from that target system when the next foreground refresh happens.

Now the redeploy feature is really just, it's not actually going out and forcing all clients to redeploy the application at that moment.

It's setting a flag on the package in Active Directory that at the next refresh, the client sees and says, oh, I need to rerun the installation for this package.

And so we'll use that redeploy in a demo in a short bit to show you how you can essentially patch that Adobe Acrobat Reader package that I just had deployed.

---

## Patching an Existing Deployment

Okay.

So now what scenario I'm going to deal with is I get a patch of Acrobat Reader from Adobe.

And I want to deploy that patch or that update to Reader to all my systems that have gotten Acrobat Reader installed via Group Policy.

So, the first thing I need to do is I need to patch my administrative install on my DFS Share and that's what this command is doing.

So, I'm running MSI exec with that slash a or administrative install parameter.

I'm giving it the path to the MSI file up on my DFS share and then I'm giving it a slash P and the name of the Microsoft installer patch file that I want to execute.

So, I'm going to go ahead and run this command and what it is going to do is, it is going to go ahead and walk me through patching this install.

And essentially, it's going to apply that patch to the existing installation that I have up there and that will be the first part of what I need to do to make this to get this deployed to all my systems.

So once that's done.

I'm going to go ahead and go into my GPO where that package has been deployed.

And I'm going to select to redeploy the application.

And what this does is it sends a, sets a flag in the GPO that triggers all of the clients that have deployed this application to know that they have something else that they want to do with that application.

They want reinstall it.

And you'll see here that the warning is redeploying this application will reinstall the application everywhere it is always installed.

Do you want to continue? So Yes, I want to continue.

And now, essentially what it's going to do is set that trigger for my entire environment.

Now what I'm going to do is come to my Windows 7 client and go ahead and run gpudate again, just to make sure that I get all of those updates from the policy.

And it's telling me that the group policy client side extension software installation needs to essentially be processed at log on, so I need to log off.

So, I'm going to go ahead and do that.

And now I'm logging back onto my system.

And what you'll see here is at logon, it's reinstalling Adobe Reader.

So instead of waiting for me to do something, it's actually patching Adobe Reader at the log on cycle.

So, essentially I'm getting the update before I even get to my desktop.

Which is a great thing.

It ensures that the user is always using that new version of Adobe Reader that they've been deployed.

And it makes sure that I get my patch as soon as I, as my administrator has made it available to me.

Now that that's completed, what I can do is I can go in here and fire up Adobe Reader.

And what you'll notice is, if I go into the About Adobe Reader XI, I now have that 11.0.09 version which is the version that I had patched up to on my administrative share.

So group policy has done its job and managed that life cycle of getting me that patch that my administrator has delivered to me.

---

## Summary

So I now I want to summarize what we learned about software deployment and group policy in this module.

So, GPSI, or Group Policy Software Installation, provides a really basic software deployment, software distribution feature that allows you to install MSI packages on a per-computer or per-user basis.

So, again, you're limited to those MSI packages, and there are obviously a few flexibilities that you have within that per computer and per user deployment around publishing or assigning.

But, by and large, it's a pretty limited capability.

So, you can publish per user or you can assign per user and per computer and in either case the user must log on or reboot to get that new package installed.

Now, when you're deploying packages you should definitely use a DFS share if possible.

This avoids that issue that I mentioned where servers that you've got packages on get retired and the sort of link between the client that's installed that application and the original share package is lost, and you can no longer upgrade or manage those packages through group policy.

So, DFS kind of abstracts the physical server from that deployment path and allows you to trade out servers as needed underneath the DFS share and the client is none the wiser in terms of knowing which server it's getting its package from.

So I talked about lifecycle management of packages using GPSI, and I mentioned that and even showed that you can patch and upgrade applications, but you definitely want to use an administration install of the MSI file, as long as it's supported.

Because that allows you to do that patching that I showed.

Without that administrative install, you can't really patch a deployment.

So it's important that if you deploy an MSI file, use that slash a option on my MSI Exec to essentially get that administrative install out to your share package.

And then finally, avoid those scenarios where the user can remove a package or try to manage an application that's been deployed by Group Policy themselves.

What ends up happening, and it's really easy to get into this situation and really hard to get out of it, is once a package gets orphaned.

In other words, either the user has removed it, but Group Policy still thinks it's there, or in the case of the share going away or the server going away the client is no longer able to be managed through that relationship.

There really are no tools in Group Policy software installation to fix that scenario.

So you really have to keep that relationship between client and server clean, and let Group Policy do all of the life cycle management.

If you don't let Group Policy do that life cycle management, then you end up in these kind of orphan scenarios, where you can no longer manage that package.

So that kind of concludes what I wanted to talk about with software deployment.

In the next module I'm going to talk a little bit about user and settings and data management and how Group Policy can help with that.

---

