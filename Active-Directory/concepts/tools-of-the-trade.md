# Tools of the Trade

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Tools of the Trade |
| Type | Automated Transcript Synthesis |

## Installing the GPMC

Okay.

In this module, we're going to talk about the tools of the Trade of Group Policy Management.

And what that really gets down to is the GPMC that I've shown you examples of already.

The GPMC is your friend when it comes to managing Group Policy.

It's really the main tool for doing Group Policy Management and you can do a lot of stuff in group, in GPMC, so we're going to dive into that a little bit in this module.

In order to get the GPMC on your desktop machine or server machine, you need to install the Remote Server Administration Tools package.

It can be installed on Windows Server or desktop SKUs.

And it's available, depending on which version of Windows you're running, it's available using different mechanisms.

So on the desktop SKUs, like Windows 7 and Windows 8 or 8.1, the RSAT tools are downloaded from the Microsoft download site.

And there are specific versions for Windows 7 and for Windows 8 and even Windows 8.1 has a different version, so you need to make sure you get the right version of RSAT.

It installs as a kind of of a feature module on .MSU package an update to the operating system.

And that gives you the ability to get at those features and install or select the GPMC.

So I'll show this in a demo, but once you download and install the RSAT that doesn't automatically get you GPMC on Windows 7 or Windows 8.1, you actually have to go in and select that feature.

On Server 2008-R2 or R 2012 or 2012-R2.

RSAT is actually a feature that you can add in Server Manager.

So if you go in under the add rolls in feature section of Server Manager, you'll be able to add the RSAT feature and then explicitly select Group Policy Management as the feature you want to install in RSAT and that essentially installs GPMC.

The other thing you can do in GPMC is you can manage other domains and forests other than the one that you've got installed on your Windows Desktop or Server.

So GPMC has this capability to manage multiple domains and even multiple forests.

There is some requirement around this, however.

You need to be trusting of the domain where you're running GPMC, so what this means is you need at least a one-way trust from your foreign domain, so to speak.

Or foreign forest to the domain in which you're running GPMC.

GPMC unfortunately, doesn't support runas for those non-trusted domains, like some of the other server administration tools do.

And this can be kind of a pain, if you're trying to manage GPOs across untrusted forests.

Essentially, you have to go to a server or workstation in that untrusted domain and manage it from there.

And if you're going to manage other domains GPOs from GPMC, you need at least read access from those domains.

And of course, if you're going to edit those domains, other domains GPOs, you're going to need write access to those GPOs.

I'm going to talk more about delegation in a subsequent module.

But essentially, you need to be able to perform those operations on the foreign domains just like you would on your current domain.

The other thing you can do in kind of a cross domain fashion is you can link GPOs across domains.

So if you have domain A and you've defined a GPO in domain A and you've got a domain B that's trusting by, a trusting of domain A and has an OU.

Let's call it the marketing OU.

You can link a GPO from domain A to the OU in domain B and that's absolutely possible.

As a general rule, I tend to avoid it because there are performance implications of doing that.

If when a user logs in or a computer starts up and has to process a Group Policy, if they have to cross a trust to read that GPO that's in domain A, then it's going to perform a little bit more poorly.

And it depends on network connections and those sorts of things, but it's not generally recommended, unless you absolutely have to do it.

You can also run into more permission issues.

If you're crossing domain boundaries, it becomes, you have to be a little bit more explicit about how you grant permissions.

That's just something to keep in mind.

I'm going to talk about in module 12, I'm going to talk about other ways of handling cross domain challenges or getting GPOs between domains and specifically around importing domains.

So now, let's take a look at some of this stuff on our test system and see how it all works.

---

## Downloading and Installing GPMC

Okay.

Now I am on a Windows 7 machine and I'm going to walk through the process of getting the GMPC installed on both a Client Operating System like Win 7 and a Server Operating System.

So the assumption is that you've gone ahead and gone to the Microsoft download site.

And in this case, I did a search on RSAT and you'll see here that the first entry is Remote Server Administration Tools for Windows 7 with SP1.

I'm going to go ahead and I've already installed this.

But if you were to go ahead and click this and run this install, it will perform a feature upgrade and add the Remote Server Administration Tools.

Now once they're added, as I mentioned in my previous clip, the GPMC's not actually installed yet, it needs to be added.

So what you do is come into the Add, Remove Programs > Control Panel applet and go to Turn Windows features on or off.

And keep in mind, this is the same for both Windows 7 or Windows 8 or 8.1.

So it, they both work basically the same way.

And once that comes up, what we're going to do is drill into the Feature Administration Tools and.

Okay.

So now, we've got Feature Administration tools under Remote Server Administration Tools and in Featured Administration Tools, you'll see here that Group Policy Management Tools are checked.

Normally, this would be unchecked right after you install the RSATs.

You would come in here and check this and it would make the change and add GPMC to your system.

So you'd be good to go and you can just come to your command prompt and type gpmc.msc or you can go to Administrative Tools that actually aren't on this Start menu.

But if you had Administrative Tools in the Start menu, you could see Group Policy Management.

That's one of the options.

Now if we shift gears to a Server 2012 box, if I'm in Server Manager, which is sort of the, you know, the configuration dashboard for Windows Server as of 2008 R2 and come in under Management > Add Roles and Features.

Click Next.

Click Next.

Click Next.

Click through the Server Roles and once you get into Features, you'll see Group Policy Management as an option.

Now, this is a active directory domain controller and GPMC gets installed by default when you promote an active directory domain controller.

If this were just a member server, you could come down to this Feature list and just click on Group Policy Management and you would get that, the GPMC installed.

So that's really all it takes to get GPMC on your systems for managing Group Policy.

You know, in terms of which is, which platform is better to manage Group Policy.

I know a lot of IT shops that don't like administrators logging into servers.

So, there's a, tend, tends to be a preference of installing and, and doing group policy management from a Desktop's queue, like Windows 7 or Windows 8.

I think it really just depends on your own best practices.

I don't think one is better than the other.

Some folks like to have all of their admin tools on their server box.

And they'll use that as kind of the place to go to do administration.

I think that's perfectly reasonable if your policies allow that.

So, let's shift gears and talk a little bit about the cross domain stuff that I had mentioned.

So, GPMC, as I mentioned, has the support for cross domain management.

And what you can see here is that I've got the, the forest that I am currently installed in is R2Test.tld.

Okay, so what I am going to do is right click on the Group Policy Management note on the top there.

Select Add Forest, and I've got the name of a forest that I'm trusted with here, called sdmtest.net.

And as you can see, it adds the forest, and then I have the ability to browse it's GPOs it's OUs, and manage it just as I would manage the my own domain and forest.

I'll also mention that if you click on the Domains node, and say Show Domains, if you are in a multi domain forest, you could selectively show or hide domains in that forest using this dialogue.

So there is a lot of options here in terms of managing multiple domains.

And again you have to have permissions to be able to edit domains on this foreign forest.

You'll note the Edit option is blanked out in, in this, for this particular GPO.

And that's because even though I can read the GPO's and read the AD structure in sdmtest.net.

I cannot actually write to or edit those GPOs, unless I'm explicitly given permissions to do so in that domain.

So that's just, a, a, something to keep in mind as you're managing, you know, remote domains through GPMC.

And again, remember that GPMC requires those trusts.

It doesn't support this notion of a run as, like some of the other active directory related tools do.

So you need to have trusts to all the domains that you're managing in order to actually read and, and potentially write to those GPOs.

---

## Navigating the GPMC

Okay, so now that we've, sort of learned how to get the GPMC installed, and, some of its basic capabilities, what I want to do is start, digging into the features and functions of the GPMC.

So, you know, GPMC is really designed to be a GPO centric view of AD.

It really isn't a replacement for your existing AD management tools like AD users of computers.

But rather is designed to give you that kind of group policy view into your active directory.

So it shows you basic AD structure, but it does not show computer or user accounts.

So you can't really see within an OU which computers or users are in there.

You have to drop back to something like AD users or computers.

You can have a view of all the GPOs defined in a domain and their properties.

You can also see where those GPOs are linked and you can do some basic reporting and troubleshooting around group policy objects.

So, you know, the bottom line here is that GPMC is not a replacement for your AD tools.

It's sort of complimentary to those AD tools.

And you'll find yourself bopping back and forth between the two, as I do on a regular basis.

Now, let's kind of dig into the GPMC itself and look at all the parts and pieces to it.

So the first thing to note is, of course, the kind of unit of work is the domain.

And in this screenshot, I've got a single domain in my r2test.tld forest, and it's called r2test.tld.

And underneath that domain, it shows me the AD structure of that as well as where those GPOs are linked.

And that, it has this sort of special container that's, if you were to bring up AD user as a computer, you wouldn't see a container called Group Policy Objects.

But GPMC sort of provides that view to you.

And it's essentially that container contains all of the GPOs that are defined in this particular domain.

So you can kind of at a glance get a list of those GPOs, and even see properties on them.

And the properties, if I've selected a GPO, either underneath the Group Policy Objects container, or if I go up, let's say I wanted to look at the Default Domain Policy.

I can select it in the Group Policy Objects container or I can select it in the Default Domain Policy.

I'm sorry, in the, in the domain container, I could select the link.

And, it would show me the same view which is a set of, essentially properties on the GPO, everything from where it's linked and how it's security filtered to other information that I'll get to in a bit.

So lets go look at this in real time and see what this looks like on our test system.

---

## Navigating the GPMC

Here I am on my GPMC console on my Server 2012 box and as you can see I've got my r2test forest.

Now, I can go ahead and as I showed before, show the domains that are available in this forest and add them as needed.

In this case, I only have one.

And what you see is, as I select each container, in this case, the domain, it shows me the GPOs linked to that container.

And I can actually see those over here, and change their order.

Remember, I talked about order in the previous module, that controls, GPO processing.

So I can essentially, show, as I click on each one, I can see the GPOs that are linked to it.

If I select the actual GPO, either from the link, which is what I'm doing here, or if I expand group policy objects and select the same GPO here, I get the same view over here on the right.

So there's no difference between selecting the GPO here as a link, and selecting the GPO within the Group Policy Objects container.

And that's, that's really a subtle yet important distinction to understand.

And how it's different is that, if I were to come up here and click, right click and say Delete.

What I'm deleting here is the link, not the GPO.

However, if I come down here and right click Delete, I would actually be deleting the GPO, that's an important distinction.

So within the Group Policy Objects container, if I need to deleted GPOs, this is the place I'm going to come to do it.

And this is how I do it, essentially highlighting a GPO, right clicking, and saying Delete.

So again, we've got the domain, we've got all of our OUs.

You'll notice that there are no computers in here, there are no users, so I can't see what's in this OU that these users or computers are going to get.

But it still shows me, you know, exactly what GPOs are linked to which OUs, and it shows me the OU structure.

---

## Understanding GPO Properties

Let's dig in a little bit deeper on the, GPMC now.

So, I've shown you that kind of basic view, that main view, now let's dig into the properties around GPOs.

So, what you see here on this screen is the Details tab for the GPO and essentially.

What we've got here are a number of different properties like number of changes on the GPO.

The GPO GUID, whether you can enable or disable the GPO and then GPO comments, if any.

So it provides you with a lot of information about the GPO kind of at a high level.

I call this the sort of GPO metadata.

Created date, modified date, who the owner is.

Again, user version and computer version.

Those are related to how many changes have been made on the computer and user side of the GPO.

Now, if we shift to the scope view, this shows us a little bit different information.

This shows us how the GPO is linked and filtered.

In this case, this GPO, the defaulter's main control policy is linked to the domain controllers OU.

So it shows the list of links and their states and of course a GPO can be linked to multiple containers so this list could be more than one item here.

But in this case it's only showing us one container where the GPO is linked Security Group filtering.

In this case, the default Security Group filtering is in place, but this could be other computer or user groups or user or computer accounts, individual accounts.

Remember, I said that this is the dialogue you use to control any security group filtering on a GPO.

And then WMI filtering is listed down here, and it will show if there are any WMI filters linked to this GPO, it will show those WMI filters.

And then the GPO settings tab shows us, the actual settings that are defined in the GPO in this kind of html format.

And if you right click anywhere in this report, it will allow you to save it as HTML or XML.

So that you could potentially save this off and use it at another time to look at, the settings that are contained in this GPO.

And then finally this is a little bit off the, the properties of the GPO, but within the view menu in GPMC, there are a set of view options on the data within the Group Policy Management Console.

And I've sort of focused on these three because they tend to be the most interesting and the most often used.

The first one is checked by default, Enable trust detection.

And this just lets you limit the domains that are shown or the forests that are shown to only those that have two-way trusts.

If there's only a one-way trust, then they wouldn't be shown in in the GPMC console.

The second option, show domain controllers after domain names, is probably one of the ones that I use most frequently.

And when you enable that, as I'll show you in the next demo, it gives you the ability to see the domain controller that you're connected to.

And performing operations against when you're using the GPMC and subsequently the GP Editor.

Just a note by default all GP management operations are typically done against the PDC emulator domain controller In the domain.

So that's the default state.

You can actually come into GPMC and change that.

And I'll talk about that in a later section.

But this is essentially showing you which domain controller you're connected to.

And then, this last Show Confirmation dialog.

That is, you'll get a warning whenever you performing operations on GPOs or GPO links.

To let you know that the operation you're performing is perhaps on a link rather than on the real GPO.

And this will come up if you'll remember I just mentioned that if you try to delete a link, all it's deleting is the link, not the GPO.

And that warning will come up if this box is checked, whenever that kind of an operation happens.

---

## Understanding GPO Properties

So, now I'm going to review what we just talked about, just so it's crystal clear.

Let's see.

The GPO that we're focused on right now, default domain controllers policy, oh by the way, if I click into this, I can change the name of this GPO just as soon as, I type in a new value.

So, just be aware of that.

It's easy to change the name of a GPO in the GPMC.

There's really no downside to doing it, because everything that, clients care about in terms of processing GPO is tied to that GUID, which is a good segue into talking about the Details tab.

The Details tab provides a bunch of information about this GPO, what I called in my previous section the metadata.

So here's the guid of the GPO, this is the unique identifier that never changes regardless of what the name of the GPO might be.

So this value is always constant through the life of the GPO.

And, in fact, the two GPOs that Windows creates when you first promote an Active Directory domain controller into a new domain are the default domain controllers policy and the default domain policy.

And they always have the same GUID, so the GUID that you see will be the same for the default domain controller's policy in this domain or any other domain running Active Directory.

Now, the version information I mentioned briefly in my previous discussion and what this relates to is it tells you how many changes have been applied to each side of the GPO.

And the number that you're interested in is really the, the numeric value, the fact that it shows the AD side and the SYSVOL side is something that we'll get into a little on in this course.

But essentially, Active Directory GPOs are really in two parts.

One part is in the AD and the other part is in SYSVOL.

And the SYSVOL replicates those around, just as AD does.

And so this is telling you that there's a version number kept on the SYSVOL part of the GPO and a version number kept on the AD part.

And those are in sync, which is what you want to see.

You want to see these both being the same number.

in this case, on the computer side, you can see that one change was made to the computer side of this GPO and that it exists as number one or version number one in AD and SYSVOL.

And then of course we've got Last Modified Date.

This tells you when the last time a change was made to the GPO and the day the GPO was created.

The Owner Information, this gets populated automatically by whichever user or group has created this GPO so it's pretty straightforward.

And typically, you don't change that, you don't go in and change the owner.

There's, there's no real reason to.

Under the Scope tab, remember that I said that we list the links that this GPO has.

So in this case, it's linked to the domain controller's OU.

The link is enforced or not enforced and the link is enabled.

And this is the path, the full path, to that OU.

Remember when I talked about enforced, that means that this link is not forcing down these settings on.

This OU and anything underneath this OU.

So, if I right-click this, I could very easily set this to enforce.

I can also disable a link.

This is not quite the same as deleting a link.

So, if I disable a link, effectively, the GPO is ignored, that's linked to this container.

But it's not completely gone as if I were deleting it so I can come back in.

Let's say I want to test something.

And I don't want this GPO to take effect on the domain controller's OU.

I can temporarily check this box to disable that link and then come back in later and enable it again.

So it's a simple matter of, sort of temporarily turning off the GPO on this container.

Again, the security filtering.

I talked about this in a previous module.

But this is showing what security filters are on this GPO.

And what WMI Filters are linked to this GPO.

Remembering again, that only one WMI Filter can be linked to a GPO at a time.

And then finally, the Settings Report.

Again, this is the report that shows you all of the settings that are applied or that are defined within this GPO.

And it shows them in sort of that hierarchical format that you would see in GP Editor.

So you can drill in and look at whichever policy area you're interested in.

This one only has computer side settings, but if this were another GPO with the user side settings, they would be listed here just like they are here.

And as I mentioned, you can right click on this.

You can edit the GPO.

You can print this HTML report, or you can save the report.

And it gives you an option of saving it to either HTML or XML, depending on what you're trying to accomplish.

In most cases, if you save it as HTML, then you can publish it as a HTML file on a website if you wanted to.

Now you'll remember that I also mentioned this View Options menu.

In the GPMC.

And let's just go through that quickly.

So, I've got, I can do some control of what is displayed in each of the different views, within the Group Policy Management Console.

I can also make some changes to how reporting is done.

This first option relates to the old .ADM files, these are the admin template files that preceded ADMX.

Normally they're going to look in the default local folder then in SYSVOL for these ADM files.

The ADM files are used during reporting to resolve.

Registry values to their friendly names as they appear in GP Editor.

You can also have the option, I haven't talked about having administrative template comments but there is the facility to actually put comments within.

Only within Administrative Template settings.

So you can go ahead and view those in the GPMC Settings reports if you wanted to using this check box here.

And then, again, going back to the General tab.

This is where I showed you some of these options for showing domain controllers.

Showing confirmation of the distinction between links and GPOs.

I'm just going to go ahead and check this one here so you can kind of see what happens.

So, you'll notice here that the display changed, and now it's showing me which DC I'm focused on within GPMC.

This is useful if you have a large number of DCs and you want to change DCs that you're focused on or at least see what you're focused on at any given time.

I can actually change it by going here and saying Change Domain Controller and in this domain I only have one DC but if there are other DCs in this domain, I could pick one.

Choose this and pick that DC, and I would be focused on that other DC.

And that would mean that any edits that I did to GPOs or any reporting I did were done against that other DC.

---

## Creating and Editing GPOs

Now let's shift gears a little bit and talk about creating and Editing GPOs.

So as I sort of elluded to GPMC is again, your main tool for creating and deleting GPOs.

Now when it comes to editing GPOs, what happens is you launch this GP Management Editor GPME, out of GPMC.

And I'll show you that in a bit.

It's a simple process.

But that's really the only way to launch the GP Editor against a domain based GPO is to do it out of the GPMC Management Console, or GPMC.

Now when it comes to creating a GPO in GPMC.

There's a couple of ways to do it.

You can either select that Group Policy Objects container, and then select "New" after right-clicking it.

Or you can right-click any container object in the domain, so a site, domain or an OU.

And then, select the option that I've shown you before to create a GPO in this domain and link it here.

So in both of those scenarios, you're creating a new GPO.

In the second scenario, you're creating it and linking it in one operation.

So it'll still be empty because it's a newly created GPO, but it'll also be linked to that, whatever container you're focused on.

So editing GPOs are really a simple matter of right-clicking the GPO.

And you can do this from either the link as it appears under a container object.

Or under the Group Policy Objects folder.

So you right-click it.

Select the very first option which is Edit.

And it does presume that you have Edit permissions on that GPO.

I'm going to go into the delegation parts of Group Policy Management in a little bit later in a future module.

But essentially.

Assuming that you actually have Edit rights on a GPO, which usually are granted to the Domain Admin's group, members of the Domain Admin's group, members of Group Policy Container owner, I'm sorry, Group Policy Creator owners, and Enterprise Admins.

So those three groups if you're in one of those, you'll have Edit defaults on the domain GPOs by default.

So right-clicking and editing pretty straight forward.

Now adding a comment to a GPO.

I showed you in the previous section, how you can display comments from the details tab of a GPO.

You actually can add the comment and the only way to add the comment, well, there's two ways.

One is from the GP Editor.

Any other is from PowerShell which is the Microsoft scripting language.

And much later in this course I'll be going over how you can use PowerShell to perform various group policy management tasks.

But in any case, the way you typically do this is, if you're in GP Editor you right-click on the GPO name in the left hand pane where the GPO name applies or where it shows up.

Choose Properties and then select the Comment tab as you see in this screen shot and then you can type free form text into this.

Now I tend to like to put a little structure behind it.

I'll use the Comment field to put in dates and times of when I made a change and what the change was for.

And usually sign the change with my name so that people know who come along to this GPO, what's been going on on this GPO.

It's kind of a, poor man's change control system but it works just fine for a lot of situations.

---

## Creating, Editing, and Commenting GPOs

So let's see what it takes to do some creation, deletion, and editing of GPOs.

So I'm going to just go ahead and right-click the Group Policy Objects container and say, New.

And I'm going to give it a name.

I'll call it my Lockdown GPO.

And one thing to note is that even though as I mentioned earlier, the name is not consequential in terms of what matters in a GPO.

It's really the.

You still can't through GPMC create two GPOs with the same friendly name.

You'll get an error when you try to do that, in fact if I go ahead and try to create a policy called Lockdown Policy now You'll see that it tells me that the name already exists and I need to choose a different one.

So that's just something to keep aware of.

Now I can go ahead and delete this GPO as simply as clicking Delete.

And it tells me that all the links.

That exists for this GPO so if this GPO has been linked to container objects in this domain, they'll be deleted.

Those links will be deleted.

If those links exist in another domain, and remember I said you can link cross domain if you choose to do so, well, those links will not be deleted.

So, they'll essentially be orphaned if you delete the GPO.

And, you'll have to go back and clean those links up in the other domain.

So, I'm just going to go ahead and delete that GPO I just created.

Now, remember I said I can just simply right-click on a GPO, and choose Edit.

And, if I go ahead and edit a GPO, it brings up the GP Editor.

And, I can go in and edit it, as I've showed you in the past.

You can drill into any of these containers, and do some work on them.

Now let's look at the comment capability.

If I right-click on the name of the GPO, up in the top here, and choose Properties, and then Comment.

So, I can now put in a comment, and I'm going to say, this is my test comment on the Dallas Sales Printers GPO-Darren.

And, that comment is now baked into that GPO.

If I go back to the GPMC, and look at Details tab, you'll see my comment shows up right here.

So, it's pretty straight forward to add a comment to a GPO, and obviously very straight forward to simply edit a GPO, assuming that I have permissions to do so.

Okay, let's go ahead and wrap up this module, and we'll talk in the next module about how group policy gets applied at target systems.

---

## Summary

Let's summarize this chapter.

We started off by learning about how to get the GPMC installed as part of the Remote Server Administration Tools.

Remember, that you can download this standalone RSAT package for either Windows 7, or Windows 8 versions.

But, it is a feature in any of the Server SKUs.

So, you could run Server Manager, and add it through the Add Features and Roles function.

Once you've got GPMC installed, you can use it to manage multiple domains and forests as long as there's trust in between them.

So, remember that you can potentially link GPOs across domains or forests, although I mentioned there are some performance downsides to doing so.

Now, GPMC provides kind of a GPO-centric view of Active Directory, so it's really not meant to replace, or be used instead of tools like AD Users and Computers.

But, it is a great tool for giving you that GPO-centric view.

Again, it doesn't show computer objects or user objects in AD, but it does show the OU structures, the sites that are available, and containers you care about when it comes to linking and targeting GPOs.

So, it's really kind of the hub for group policy management, for creation, for editing, linking, reporting, and trouble-shooting.

And, as far as creating and editing goes, GPMC is the place you start when you need to create new GPOs or edit GPOs.

From an editing perspective, when you right click a GPO and choose Edit, it launches this GP Management Editor.

Or, I simply call it the GP Editor.

Focused on the domain based GPO, and let's you make changes to settings.

And, you can also, from that GP Editor, add comments to the GPO.

So, you can see sort of what kinds of things have happened on that GPO.

You can use it to annotate changes to the GPO, or whatever you really want to use it for, it's a free form text.

Then, you can view that from the Details tab, once the GPO is highlighted.

And, just to kind of finalize that thought, remember that the GPO has a number of tabs available to it that provide everything from those metadata around the GPO, like how many changes have been made to it, when it was last modified, what it's status is.

To the settings report that lets you generate an HTML or an XML report of all the settings that are available in the GPO.

The scope tab that lets you see the targeting of the GPO, so where it's linked, the security group filters, or WMI filters that are applied to it.

So, it's really kind of a complete place to go for most of your GP management tasks.

---

