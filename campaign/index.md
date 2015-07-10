---
layout: tutorial
title: Custom Campaigns
warning: This tutorial is under construction. Sections may be incomplete or missing info. Continue at your own risk.
---

For a this tutorial you will need:

* A capable computer with Homeworld Remastered installed (duh)
* A text editor (I highly recommend using [Notepad ++](https://notepad-plus-plus.org/))
* Basic knowledge of [Lua 4](http://www.lua.org/manual/4.0/) 

## Comparison of Campaign Loading Methods

If you just want to get to modding you can skip this section.

As of the June 9 update custom campaigns can be loaded in three different ways

* Overwriting the ascension campaign
* Specifing the campaign with the `-campaign` parameter
* Launching the campaign from the Extra Missions menu

Here is a brief comparison of the pros and cons of these methods:

Method of Launching | Preserves the default save | Doesn't require launch parameters | Launching directly from the main menu
-:|:-:|:-:|:-:
Overwriting the ascension campaign | <i class="fa fa-times fa-2x"></i> | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-check fa-2x"></i>
Specifing the campaign with the `-campaign` parameter | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-times fa-2x"></i> | <i class="fa fa-check fa-2x"></i>
Launching the campaign from the Extra Missions menu | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-times fa-2x"></i>

Launching from the Extra Missions menu provides the least hassle for the end-user (the person playing your campaign) so that will be the method covered in this tutorial.

## Creating a mod shell

Mods in Homeworld Remastered work by overiding or adding to the files found in the default .big archives with your custom data.

We will need a place for these files to live in while we're working on them.

Locate your Homeworld Remastered install folder. If you don't know where this is you can find it by going into Steam, right clicking on Homeworld Remastered Collection, and then Properties > Local Files > Browse Local Files > HomeworldRM. You'll want to note the file path as we'll need it later. 

Create a folder here for your mod. You can call it anything you want, but if you are following along with this tutorial, I will call mine `CampaignTutorial`.

Next up, inside this folder create a file called `keeper.txt`. You can put anything you want inside this file, so I will just leave mine blank. It is important that this file exists, as it tells the game that this is the root directory of your mod.

Additionally you can put a `config.txt` and `preview.jpg` in this folder, as these are required to upload to the Steam Workshop, but that will not be covered in this tutorial.

At this point the basic mod shell is finished and you can launch your mod.

## Launching the mod

The mod can be launched by passing to the game the `-moddatapath` parameter followed by the name of your mod's folder.

Launch parameters can be set in many different ways, but while you are working on your mod, I recommend that you create a custom shortcut to launch your mod.

From the Homeworld Remastered install folder navigate to `Bin\Release\`. Right click drag HomeworldRM.exe to your desktop and select `Create shortcuts here` from the dropdown menu. Right click on the new shortcut and select `Properties`. At the end of the `Target` line add a space and then `-moddatapath CampaignTutorial`. If you called your folder something other than `CampaignTutorial` use that instead.

You must make sure Steam is running before launching the shortcut or else it won't work properly.

Now use the shortcut to load up your mod and see what happens.

At this point your mod will do nothing because we haven't added anything to it. So let's fix that.

## Setting up the Campaign

In your mod's folder create a folder named `leveldata` and inside there, create a folder called `campaign`. This is where the game looks for the configuration and level files for your campaign.

You should decide on a name to use internally to represent your campaign. This should be easy to type and remember. It shouldn't be the same as one of main campaigns either, so you can't call it any of the following:

* `ascension`
* `homeworldclassic`
* `rr_oem`
* `steamws_banner`
* `tutorial`

For this tutorial I will call my campaign `tutcampaign`.

Once you have decided on a name, create a folder and a text file with that name. The text file should have the extension `.campaign`.

For this tutorial our campaign will only contain two missions. Place a folder inside the `tutcampaign` folder for each level in the campaign. To keep it simple I will call my levels `m1` and `m2`.

Assuming you are following along, your mod's folder should now look like this:

	─ CampaignTutorial\
	  ├ keeper.txt
	  └ leveldata\
	    └ campaign\
	      ├ tutcampaign\
	      │ ├ m1\
	      │ └ m2\
	      └ tutcampaign.campaign

## The .campaign file

Mods in Homeworld Remastered are writen in a scripting language known as [Lua](http://www.lua.org). The most commonly used version is Lua 5. However Homeworld Remastered uses the older Lua 4. There are a couple differences between the two, and people used to Lua 5, as well as those who have never worked in any version of Lua should be familiar with the basics of Lua 4. The best reference I could find is the [Lua Reference manual for Lua 4.0](http://www.lua.org/manual/4.0/). From this point onward in the tutorial, knowledge of Lua will be assumed.

The .campaign file tells the game information about your campaign. Let's open up your .campaign file in your text editor of choice.

There are several variables we can set to define properties about our campaign. Here are the ones you should define:

* `displayName`
	* A holdover from Homeworld 2, this would be the name displayed at the top left of the Mission Select Screen. This no longer displayed in Homeworld Remastered, but it would be best to define it, in case GBX ever decides to re-enable it. Set this as the name you want your player to see. I will set `displayName = "Tutorial Campaign"`.
* `contentType`
	* Does nothing as far as I can tell, but is supposed to indicate what type of campaign this is. Options for this are:
		* `0` is for "Primary" Campaigns. These are the campaigns that come with the game.
		* `1` is for "Tutorial" Campaigns. The in-game tutorial is the only one that should be this.
		* `2` is for "Extra" Campaigns. These are campaigns that show up in the Extra Missions menu. Raider Retreat is one of these.
	* As our campiagn is an Extra Campaign we will set `contentType = 2`.
* `contentName`
	* This seems to be the internal name of the campaign. Simply set as such: `contentName = "tutcampaign"`
* `contentOrdered`
	* This governs how levels are unlocked. At the moment there are only two options:
		* `0` - all levels start unlocked.
		* `1` - the previous level must be completed to unlock.
	* GBX will likely add more in the future. I am going to set `contentOrdered = 1`.
* `contentChooseText`
	* This is the text diplayed at the top of the level select, prompting the player to select a mission. I am going to set `contentChooseText = "$3492"`. This is a special type of string called a localised string. I will discuss this more later.
* `contentThumbName`
	* This is the name displayed on the Extra Missions menu. It should be the same as `displayName`, so set `contentThumbName = "Tutorial Campaign"`.
* `contentThumbDesc`
	* This is displayed on the Extra Missions menu, underneath the title. It is a short description of your campaign. You can set it to whatever you want.
* `contentSort`
	* This determines how far up the Extra Missions menu the campaign is placed, relative to other campaigns. The developers want this left at `1` so let's set `contentSort = 1`.

At this point your file should look somewhat like this:

{% highlight lua %}
displayName = "Tutorial Campaign"

contentType = 2
contentName = "tutcampaign"
contentOrdered = 1
contentChooseText = "$3492"
contentThumbName = "Tutorial Campaign"
contentThumbDesc = "A short campaign to teach the basics of custom campaigns."
contentSort = 1
{% endhighlight %}

As I mentioned earlier there is a special type of string known as a localised string. These consist of a dollar sign followed by a number. At runtime these are replaced by a different string depending on the language. The string we used earlier, `"$3492"`, would be replaced by `"CHOOSE MISSION//"` if the game was run in English. It would be replaced by a translated equivalent if the game was run in another language. While you can add your own localised strings, it requires extracting the game's .big archives, so it will not be covered in this tutorial.

There is one last variable to set. `Mission` is an array containing info about all the levels in your campaign. Start off by defining it as an empty array.

{% highlight lua %}
Mission = {}
{% endhighlight %}

Each member of `Mission` is an object containing various keys about one level. Go ahead and define an empty object for each level.

{% highlight lua %}
Mission = {}

Mission[1] = {
	
}

Mission[2] = {
	
}
{% endhighlight %}

Each mission has several properies that can be set:

* `postload`
	* A function which is called after the level has finished loading in. This is used to play Animatics (The hand drawn cutscenes) or NIS (Non Interactive Sequences) (the in-engine cutscenes). Since we are doing neither we can either not define this at all, or as an empty function: `postload = function () end`
* `directory`
	* This is the name of the folder the level resides in. In our case this would be either `m1` or `m2`.
* `level`
	* The name of the level file. This should be the same as `directory` with `.level` added to it. We havn't made this file at this point in the tutorial.
* `postlevel`
	* A function called after the level is complete. This can also be used to play Animatics or NIS. Unlike `postload` this function must be defined and should call `postLevelComplete()` when done. Since we aren't playing any cutscenes we can simply define `postlevel = function () postLevelComplete() end`, or even define postlevel as postLevelComplete: `postlevel = postLevelComplete`
* `displayName`
	* The name of the mission. It is displayed on the mission select screen.
* `description`
	* This is displayed under the name on the mission select screen.

Here is an example of how your `Mission` array should be defined.

{% highlight lua %}
Mission = {}

Mission[1] = {
	postload = function () end,
	directory = "m1",
	level = "m1.level",
	postlevel = function () postLevelComplete() end,
	
	displayName = "Mission 1",
	description = "The first mission of the Tutorial Campaign",
}

Mission[2] = {
	directory = "m2",
	level = "m2.level",
	postlevel = postLevelComplete,
	
	displayName = "Mission 2",
	description = "The second mission of the Tutorial Campaign",
}
{% endhighlight %}

At this point your .campaign file should look somewhat like this.

{% highlight lua %}
displayName = "Tutorial Campaign"

contentType = 2
contentName = "tutcampaign"
contentOrdered = 1
contentChooseText = "$3492"
contentThumbName = "Tutorial Campaign"
contentThumbDesc = "A short campaign to teach the basics of custom campaigns."
contentSort = 1

Mission = {}

Mission[1] = {
	postload = function () end,
	directory = "m1",
	level = "m1.level",
	postlevel = function () postLevelComplete() end,
	
	displayName = "Mission 1",
	description = "The first mission of the Tutorial Campaign",
}

Mission[2] = {
	directory = "m2",
	level = "m2.level",
	postlevel = postLevelComplete,
	
	displayName = "Mission 2",
	description = "The second mission of the Tutorial Campaign",
}
{% endhighlight %}

Load up the game and make sure everything looks right. Fiddle around with the settings to see what they do. If you try to load any of the missions, the game will crash.

## AAHH! Why is it plaid?!

That's the default picture the game uses for thumbnails. I don't know why it's plaid. We can just leave it for the time being. Don't worry, we'll change it later.

## 
