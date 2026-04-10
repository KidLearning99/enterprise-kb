# Policy Scenarios - Folder Redirection, User Settings, and Data

## Metadata
| Field | Value |
|---|---|
| Source | Pluralsight: Policy Scenarios - Folder Redirection, User Settings, and Data |
| Type | Automated Transcript Synthesis |

## How Folder Redirection Helps Redirect User Settings & Data

Okay as we move along in our Policy Scenarios discussion in this module, I'm going to talk about Folder Redirection and User Settings and Data.

What are User Settings and Data? User Settings really are that stuff stored in either the registry or in the user's profile that control the User's experience.

And the User Data are things like their Documents Folder, Music, the Desktop, anything, any kind of data files or documents that the user has created and saved within their standard folder, folder structure under their profile.

Settings, again, those are things that control the user's experience and application configuration.

And they're often stored in the registry under HKEY_CURRENT_USER, and that's the registry key that is actually part of the user's profile, so, if you go in under a user profile, you'll see a file called and ntuser.dat, D-A-T.

And that file is actually the registry hive HKEY_CURRENT_USER.

And all of the per-user configuration settings for applications, installed software and et cetera are stored in that registry file.

There are also some applications especially more modern applications might use config files, like xml config files or even the old eni files, and in those cases they're often stored within the AppData or the Application Data folder, under the user profile.

So settings can take these two forms, either registry keys and values or you know, files under AppData.

Now data, pretty straight forward what that is, it's really all the user's documents and files, and these are usually stored in the user profile under Document, Music, Pictures, Favorites, etc.

It really actually depends on the Operating System version, so each subsequent Operating System version since Windows 2000 has added new kind of Favorite folders for these things, even renamed some of them.

And over time that's just grown, in terms of what you know, how Microsoft, by default, segregates your documents and your files? What is the challenge with all of this stuff? You got a user, they've got their settings, they've configured through installing applications and getting Group Policy and all the preferences they tweet.

They change their background color.

They change their screen saver etc.

What's the challenge here with this User Settings and Data stuff? Well, when a user moves from machine to machine as they maybe want to do from time to time.

They might have a laptop and they're taking their laptop home and VPN-ing into the corporate network.

And when they're in the corporate network, on the corporate network somewhere, they've got a desktop machine.

They move from each one, or maybe they're kind of work force that you know, like a call center work force that comes in and sits down at a different PC everyday.

You want to guarantee that their settings and data follow them when they sit at those two machines.

So, if I'm on the desktop today, my User Settings and Data are there and if I go to my laptop, they're there with me as well.

You know, basically this is kind of the goal really.

The challenge and the goal is to get those user experiences the same no matter where those users sit.

How does Group Policy help with this? Well, it provides Folder Redirection Policy, which is something that lets you redirect those key User Profile Data Folders to server shares.

So, Folder Redirection is strictly concerned with the data part of the User Settings and Data equation, and it is really designed, to, for example, the simplest example is the Documents Folder.

It's really designed to take your documents folder and redirect it to a server share and this accomplishes a couple things.

One is that, that Ddocuments Folder is available no matter where the user is because it's now on a server instead of on their local user profile.

Now the other thing that it accomplishes is that if you wanted to backup the user's data, instead of having to go and backup every individual work station where the user might have left their data then you've got this central server share that you could back up and it makes it nice and easy to do that.

The other piece of this is Roaming User Profiles and these have been around for a long time.

They are designed to let you redirect the settings and even user data to server shares.

The concept around user profiles.

I'm not going to go into too much here.

Because it's sort of outside the realm of the policy.

But the concept of user profiles has always been that you tell, for a given user object, you tell it where you want to store the user's profile.

The user logs in and it downloads their profile from that server share and the profile again contains their settings and potentially their data like Documents, Favorites etc.

Now when the user logs off, any changes that they've made to their profile, maybe they've changed their background color or whatever have written back up to the server.

And so they're available at that next machine the user logs onto.

Now, unfortunately, Roaming Profiles have been really problematic technically over the years so using roving profiles in conjunction with Folder Redirection helps get you to sort of the best of both worlds.

You can essentially redirect those key folders, get them out of the User Profile Folders like Documents Folders like Pictures, redirect them to server shares.

It takes them out of the user profile and then the stuff that has to roam during that log on, log off cycle becomes much smaller.

That makes the Roaming Profiles feature a lot less problematic.

Now, the final piece of this puzzle is that there's this feature in Windows called Offline Files.

Offline Files lets you basically work like the name says offline if a servers not available.

Now when you redirect a folder data out of a users profile to a server, that Offline Files feature is automatically enabled.

So, all of these redirected folders are automatically set for offline cashing.

So, if the sever share becomes unavailable, or the users on a laptop, and they're traveling outside the corporate network, well, they can still get access to their data.

What's the Goal here? When users roam, you want to make their experience the same as their normal machine.

So you want to Synchronize their settings to each machine.

Synchronize it as little as possible to each machine.

And Redirect as much user data as possible to ensure that you're not dragging tons of data from machine to machine.

---

## Understanding the User Profile

Okay.

Now what I want to do is sort of bring home some of these concepts that I've just been talking about.

Let's take a look at the user's profile.

The way I'm going to do that is I'm going to just type, %userprofile% here.

And what that does is it actually brings up the contents of the user's profile as it's seen from the file system perspective.

Now, another way I could get to this.

I could go in under computer.

(C:) Users, and then the User name and, here it is in a different format.

You'll see here this file ntuser.dat.

Now remember I mentioned that that is, essentially the HKEY_CURRENT_USER hive.

So that is, if I bring up the registry editor That is this set of keys here, that file underscores, or it is the backing store for this set of keys, so it holds all of the settings related to the registry, and now here are my various folders.

You've got My Documents, you've got AppData, you've got the Desktop, you've got Downloads, Favorites, et cetera.

These are all the various folders that represent the data part of the user settings and data equation, so this is the stuff that I'm going to be manipulating, using folder redirection to, in order to you know, sort of have a way of managing this data as the user roams from machine to machine.

And again, this is all just standard Windows stuff and what we're doing is using Group Policy to sort of manipulate how this follows the user around.

One thing I want to show you is I'm going to go ahead and bring up my server, and, again, I mentioned that even though this isn't in the realm of Group Policy so much, I'm going to talk a little bit about roaming profiles.

Roaming profiles are defined by, at the user object in active directory, if I bring up this particular user account and go to the Profile tab, what I could do here is set a Profile path, and in this case, I have a DFS share called users, and profiledata, and then %username%, and that's actually going to fill in the user's name, and that is all you need to do to set up roaming profiles for a user.

Now, once the user logs off and logs back in again, they're going to get their profile from the server, and essentially use roaming profiles going forward, so it's a nice little feature to use, and I'm going to talk a little more about it in a bit, about how you can use roaming profiles in conjunction with folder redirection.

---

## Implementing Folder Redirection

So let's dig into folder redirection a little bit more.

So some of the features in folder redirection, and I think the first thing to know is that it is a per-user policy only.

So it's only assignable under User Configuration, and in this path policies, Windows Settings\Folder Redirection.

You know, it really does two main things, folder redirection redirects those folders out of the user profile path and into a server share.

So it essentially says, for example, with My Documents that whenever you open My Documents, whether it's from Word, or through the Explorer, it's going to really be pointing to that server share, so whatever server share you assign to the document's path, that's where My Documents ends up pointing at.

So when you save files, they're being saved to the server rather than in the local profile.

The other thing that folder redirection does for you is it creates and then moves any existing data that the user has in the local profile, in the local folder path in the user's profile, to the server share, and it does that when the user first logs in after folder redirection policy has been assigned to them.

So essentially at first log on, if the user has 10, 20, 100 meg of data, a gig of data, folder redirection policy is going to take and move that up to the server share, where you've designated for that particular folder redirection, and that does mean that that first log-on could take a while if the user has a lot of data.

Now the other thing that folder redirection provides is two modes of redirection.

There's a basic mode, which essentially says that anyone that processes the policy gets redirected to the same relative path, relative server location, I'll show you this in a bit.

That doesn't mean that everyone's data gets sent to the same folder, it just means that every user is being directed to the same server share.

Advanced folder redirection lets you designate different paths, different UNC paths or different folders based on security group membership.

So the scenario here might be that you have a file server for the sales users, and a file server for the HR users, and you obviously don't want sales users being able to access any shares on the HR server, because of the sensitive nature of the data.

So you can designate two different redirections for, let's say, the Documents folder within the same folder redirection policy and that's really all it is is that ability to designate different redirection locations based on folder path, or, I'm sorry, based on group membership.

So this screenshot shows how you can set up folder redirection, and you'll see that it's selecting the basic or advanced redirection, and then you can define where the redirected folders go, so there's a number of options here, which I'll show you in just a bit, but in this case, it's creating a folder for each user under the root path that's designated below.

In this case, it's a DFS share for user data that's called r2test.tld\User\UserData.

Now, I tend to like DFS as the place to redirect documents just like I liked it in software installation policy because it abstracts the physical server location.

So if you ever have to retire a server, a server runs out of disk space, for whatever reason that server goes away, you don't have to come in and change all of these policies, and for that reason and for the same reason that I liked it in software installation policy, I typically use DFS shares, or folder redirection, and again you don't have to necessarily replicate data.

You know, part of the value of DFS is that it has this ability of replicating data to multiple replicas, you don't necessarily have to do that.

If you choose, for example, with user data not to replicate, that's completely fine, it can exist on a single server still, but it's referenced based on this kind of domain-based relative path.

Now, the Settings dialog on folder redirection gives you control over some of the other more kind of granular options when you're redirecting this particular folder.

So for example, you can tell Windows to re-permission the folder that gets created for the user such that only the user can access it, grant exclusive rights to that folder, and in this case, the Administrator would actually have to take ownership of that content in order to see it.

So this is sort of the default mode, and it actually sets the permissions on the folder that gets created that, basically, only grants the user who's accessing that folder access to those files.

The move the contents of Documents to the new location, this is essentially that automatic copy that I talked about, you don't have to do this, you could manually copy the content for the user, but this is the default behavior for folder redirection is to copy the data from the local profile to the server share at first log on.

This down level policy checkbox, if you still have any of these versions of operating system, Windows 2000, XP, Server 2003, checking this ensures that the documents or the folders that you're redirecting will also apply to these down level clients because Microsoft made a change in Windows Vista and above to folder redirection.

And then the final policy removal controls what happens when the GPO that's delivering this folder redirection policy no longer applies.

And in this case, what's happening is that the folder is being redirected or copied back to the local profiles.

So the, essentially the location for the documents folder goes back to the local profile.

Now some best practices.

You really need to be selective about you redirect.

There's a lot of options in folder redirection, you don't necessarily want to do all of them.

Things like AppData and the Desktop.

These can have a really negative impact on user performance if you're redirecting these to the server share.

The reason for that is because a lot of applications in the case of AppData, use that location, that folder location to store settings or transient temporary data.

And if it's going up to the server share every time, it's needing to perform those operations, then the applications tend to slow down.

And with the Desktop it's sort of a interesting phenomenon that the Desktop, when it's redirected to a server share can behave in, I guess I would say, strange ways.

In as much as there are often slow performance on the Desktop or the Desktop doesn't respond as you might expect.

In any case, I tend to avoid redirecting the Desktop folder and the AppData folder, because of these kind of user performance issues.

Now if you do use that Grant Exclusive, remember what I said that even administrators will not be able to see the content of those files.

So you'll have to actually gra, take ownership of any shares or any folders if you want to get access to that user content, so that's kind of the default behavior.

So make sure that when, users are aware that those redirected folders also use the offline caching feature, offline files feature automatically.

So if the user is getting their documents folder redirected, it's also available offline.

And this can help when they run into issues where, let's say, for example, they're on a laptop.

And they might have, be working on cached versions of the files.

And then when they connect back up to the corporate network and have access to the server, those files are synced up to the server shares.

I've seen issues where this can cause issues, especially if their working on different machines.

Maybe they're on their laptop, their making changes to a document, they're going to a desktop.

They need to make sure that those changes that they've made while they were offline sync backup before they go to the other machine and start working on those same documents.

---

## Implementing Folder Redirection

All right.

So let's go ahead and implement some Folder Redirection Policy here.

So, I've created a GPO called Folder Redirection Policy and I'm going to go ahead and edit it.

And drill into Policies > Windows Settings > Folder Redirection and you'll notice that there are a bunch of folders that can be redirected.

Now keep in mind that this is a Server 2012 R2 box that I'm editing on or if it were a Windows 8 box, it'd be the same situation.

But all of these folders are not the same if you're let's say, on an XP box, there's a subset of folders that you can actually redirect.

But let's go ahead and redirect the Documents folder.

And in here, we can define as I mentioned, Basic or Advanced Redirection.

If we were to just look at Advanced Redirection, you can go in here and you can choose a Security Group.

Let's say, sales and I've got a sales users group, I'll choose.

And you can have that user redirected to that group or members of that group, redirected to a path, let's say, Server and a share.

So let's say, it's salesusers.

And then, essentially each user would be redirected to this path that it shows here, underneath that share and if I had a different group I could d, redirect to a different share all within the same GPO.

What I'm actually going to do here though, for the purposes of this demo is to just do a basic redirection, which gives me a number of options.

I can redirect to the user's home directory to a folder under the Root Path that I specify for each user.

Redirect to a particular location.

So for example, I've used this in the past when I've redirected a folder to a, like a shared location that everyone shares, like maybe the favorites are shared by all users.

That's where this kind of area or this particular option comes in handy.

Everyone goes to the same location and it might end up being a read-only share that the user can't actually write to, but they all share the same sets of files or shortcuts or whatever it happens to be.

And then of course, you can redirect back to the local profile if you choose to do so.

So let's go ahead and choose this one to create a folder for each user under the Root Path.

So, I'm going to type in the name of a DFS share that I created.

So r2test.tld user\userdata and you'll notice here that what it's going to do is, it's going to automatically create a folder under user data for the user's name, and then a document subfolder, because we're reconnecting documents.

And one thing I did want to mention going to my Active Directory accounts.

Under the User account, you'll notice in Folder Redirection I had the option to redirect to the user's home directory.

But that's talking about is any home path that might be defined on the User account.

So, if I had a home path defined here, that's where it would end up redirecting the user.

I'll also point out that this particular user has a path specified in this Profile Path option and that's to a different DFS path.

That actually is what enables roaming profiles for the user.

Well, I'm not talking specifically about roaming profiles in this.

Since they do kind of dovetail into the ability to roam user settings and data as folder redirection does.

This is relevant and this is how you define for a given user that they have a roaming profile, by entering a path where their profile gets copied up to.

Okay.

So getting back to my Folder Redirection policy.

I've got the path to find, now I want to define my Settings.

Remember, I mentioned Grant Exclusive means that when they, when the folder gets created, only that user has access to it.

So we'll go ahead and leave that alone.

Make that the default.

I'm going to say, if the user has any content data existing in the Documents folder, I want to move it up to the server share.

And if I had any down-level clients that needed to process this policy, I would need to check this box.

I'm not going to that, but I could use that if I had still some XP in my environment.

And then on Policy Removal, I want it to redirect the folder back to the local profile.

So now I've defined my policy.

It's asking if I'm sure I don't have any down-level clients and I'll say, sure.

And now let's go to my Windows 7 client.

This is Joe Sales, who's currently logged into this Windows 7 machine.

And I'm going to just click on Documents and you'll see that this user has a bunch of files in here.

And I'm just going to go back up one level and verify that the files are all local, it's in the Users local profile.

In this case, My Documents is under C:\Users\jsales.

Now what I'm going to do.

As I did before with Software Installation Policy, I'm going to do a quick GP update on this guy.

So I'm going to say, gpupdate /target:user because I only need to update the per user configuration settings.

And it's going to do the update and now.

And actually the one thing that I forgot to do, before I can do the processing of the policy, I actually have to link the GPO.

So let's go back to GPMC and we are going to link this to the users OU under Sales.

Now what I can do is go back to my WIN7 client and do that GP update again.

And now you'll notice that it's telling me that folder redirection requires a foreground refresh.

So it couldn't complete that.

So now what I want to do, I'm going to go ahead and Log Off.

And now I'm logging back in to the Windows 7 machine and you'll see it's Applying Folder Redirection policy.

And once that completes we'll go in and take a look at what's going on with the documents folder.

So if I bring up Documents, and I right-click on it.

Right-click on Documents.

You'll notice that now it's pointing to that DFS Share.

And it's available as a shared document up on on the server.

And if I go in under any of my documents, you'll notice that they're all there.

So they're all copied up to the server.

And you'll also notice that they've been made available offline.

That's the offline folders feature that I mentioned that automatically happens when folder redirection occurs.

And if I go back to my server, and if I bring up the folder where my data is, where my UserData resides, and I click in under UserData.

You'll see that's created a folder for jsales, and under there are the documents and I don't have permission because I checked that Grant Exclusive option, so I don't have permission to that.

So essentially, what I've done is to create that Folder Redirection policy and deliver it to my client.

---

## Using Group Policy Preferences for Folder Redirection

Now the next section I want to talk a little bit about alternatives to Folder Redirection, so why would you think about alternatives to Folder Redirection; well folder redirection has some challenges.

You know, it requires that foreground synchronous refresh mode to take effect, so you potentially run into the situation where the user has to log on twice in order to have the Folder Redirection policy apply.

And it can also be problematic when you're moving large amounts of data as a function of that first logon from the local profile to a server.

So essentially it provides a great solution for very let's say moderately sized environment with not a lot of data and users that don't mind that potential second logon for it to effect.

As an alternative, Group Policy Preferences, and the registry extension in particular, can also be used to move those user profile locations to server shares.

And essentially all we're doing here, it's not anything specific to folder redirection, it's just setting some registry values for the user that point their local profile locations to server shares, the same way that Folder Redirection policy does, but just through the registry.

In a specific location and without all the bells and whistles that Folder Redirection provides.

So in other words, it doesn't move the data or create the folders, it just modifies the location of the folders.

So what that means is you know, the automatic data moving and the setting the folder to be offline automatically and creating the folders up on the server.

None of that gets done as a function of this.

What just happens is that the user's profile is modified to say that, for example, the Documents folder no longer appears under Percent User Profile Percent Documents.

It now appears up on you know, //server/share/joesales/documents.

And it's really implemented by setting values under this registry key I list here in red, HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\UserShellFolders.

Sometimes you'll even hear this referred to as the User Shell Folder path.

So what this looks like is if I'm in GP Preferences and creating a new registry item, I'm basically setting the path to.

The current registry, the current user registry path that I just defined in the previous slide, I'm setting it to all the way out to my user profile path under percent username documents, just like you see here.

So let's go ahead and see how this works in practice.

---

## Using Group Policy Preferences for Folder Redirection

Okay I'm here in my GP Editor on my GPO, that I'm going to set a Group Policy Preference for.

And I'm going to come down under User Configuration, Preferences, Windows Settings, Registry.

And click New Registry Item.

And just to make things easy on myself I'm going to go ahead and browse to the folder location that I mentioned in my previous slide, so HKEY Current User, Software, Microsoft, go down to Windows, Current Version, Explorer.

And all the way down to the User Shell Folders folder or key.

And what you'll see here are the definition of the paths for all of the special folders within the user's profile.

So Desktop, Favorites, History, and they all point to, they all currently point to local profile paths under %USERPROFILE.

What I'm going to to is I'm going to modify Documents, and interestingly, and for historical reasons, documents is under the registry key or registry value called Personal.

So I'm going to go ahead and change that.

Actually, I'll go ahead and select User Shell Folders as the key that I want to modify.

And then I'm going to select Personal as the value that I want to modify.

and actually, I could've done it from up above here as well.

If I go back into that Software, Microsoft Windows.

Current Version, lemme show you this, Explorer.

User Shell Folders.

And then go all the way down and select Personal.

It'll select the right type of it as well.

So Personal setting here, and it's a type of REG_EXPAND_ SZ.

And instead of this path here, what I'm going to say is.

I want this to be my DFS share, so \\r2test.tld\user\userdata.

And then what's different from Folder Redirection is I'm going to enter in a environment variable of %userprofile%\documents.

And go ahead and Apply that.

And that's now when a user processes this policy,.

Their Personal folder location will be shifted up to that server share.

And as long as the user's data gets to that server share, so you, in this case you'll have to copy it manually for example, or get it up there through some kind of batch copying process, then.

The user is sort of redirected in, I guess I would call it a poor man folder redirection, but it's also more simple to maintain.

So you have to do a lot of the legwork of moving the user's data and permissioning and creating the folder, but the actual redirection process is a simple registry change that's happening in the user's profile.

So, again, an alternative to folder redirection sometimes is much simpler to implement, depending on your environment, than folder redirection, and a perfectly acceptable way of redirecting certain parts of the user's profile up to server shares.

---

## Summary

Okay, so let's summarize this module on folder redirection and kind of the process of roaming user data around on server shares.

So folder redirection policy, which we introduced in this module, can be used to redirect those key user data folders out of of the profile and up to a server share somewhere.

And with folder redirection you get a lot of stuff for free, like the automatic creation of the folder, and the permissioning of the folder.

You can optionally copy the user's data up at first log-on.

So you get all of that stuff for free with using folder redirection.

And I did also mention that some folders probably shouldn't be redirected, like app data and desktop.

But, that there's a lot of folders like documents, and music, and pictures, and history that can and should be redirected if you've got users that are moving from machine to machine and need a way to drag their content along with them.

So when used in conjunction with roaming profiles, this really provides that kind of seamless, you know, go to machine, from machine to machine and have not only my user experience in terms of how my desktop and environment looks, but also the data that I need to do my work, how all that kind of follows you to each of those machines that you go to.

Now again, redirection can be either basic, which is just, you know, redirect everyone that processes that policy to the same location or use the same rules to send them to the same location.

Or advanced redirection, which let's you leverage AD security groups to redirect different groups or members of different groups to different folder shares.

Now redirected folders are automatically cached using the Offline Files Feature in Windows.

And you'll notice that if you go and right click the folder or a document that's been redirected.

You'll see that it says that it's always available off-line.

And that can be turned off using group policy, using administrative templates settings.

But by and large that's the default.

Now if you don't need that, all of the bells and whistles of Folder Redirection, you know, there are some downsides that I mentioned like it requiring a synchronous foreground refresh to take effect and the fact that if you're automatically copying hundreds of megs or gigs of data up, that may not be a great scenario for the user on that first log on.

If you don't need any of that sort of auto magic stuff that Folder Redirection does, then I showed you that group policy preferences registry tweak you can make to the user shell folders path and the users registry, that can be a nice low tech alternative to making folder redirection happen.

And speaking of GP preferences, the next module I'm going to really dig into and kind of give you an overview of using GP preferences across the board, and some of the features that GP preferences provides that regular old policies don't provide

---

