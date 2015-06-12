---
layout: tutorial
title: Custom Campaigns
warning: This tutorial is under construction. Sections may be incomplete or missing info. Continue at your own risk.
---

This tutorial assumes basic knowledge of the mechanics of Homeworld Remastered, but no prior knowledge of modding Homeworld Remastered.

Certain parts of this tutorial may be made easier by using the workshop tool, I would't know, it doesn't run on my system.

## Comparison of Campaign Loading methods (You can skip this part)

As of the June 9 update custom campaigns can be loaded in three different ways

* Overwriting the ascension campaign
* Specifing the campaign with the `-campaign` parameter
* Launching the campaign from the Extra Missions Menu

Here is a brief comparison of the pros and cons of these methods is below.

Method of Launching | Preserves the default save | Doesn't require launch parameters | Launching directly from the menu
-:|:-:|:-:|:-:
Overwriting the ascension campaign | <i class="fa fa-times fa-2x"></i> | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-check fa-2x"></i>
Specifing the campaign with the `-campaign` parameter | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-times fa-2x"></i> | <i class="fa fa-check fa-2x"></i>
Launching the campaign from the Extra Missions Menu | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-check fa-2x"></i> | <i class="fa fa-times fa-2x"></i>

Launching from the Extra Missions Menu provides the least hassle for the end-user (the person playing your campaign) so that will be the method covered in this tutorial.

## Creating a mod shell

Mods in Homeworld Remastered work by overiding files found in the default .big files with your custom data.

We will need a place for these files to live in while we're working on them.

Locate your Homeworld Remastered install folder. You can find this by going into Steam, right clicking on Homeworld Remastered Collection, and then Properties > Local Files > Browse Local Files > HomeworldRM.

Create a folder here for your mod. You can call it anything you want, but if you are following along with this tutorial, I will call mine `TutCampaign`.

Next up, inside this folder create a file called `keeper.txt`. You can put anything you want inside this file, so I will just leave mine blank. It is important that this file exists, as it tells the game that this is the root directory of your mod.

Additionally you can put a `config.txt` and `preview.jpg` in this folder, as these are required to upload to the Steam Workshop, but that will not be covered in this tutorial.

At this point the basic mod shell is finished and you can launch your mod.

## Launching the mod

by passing to the game the `-moddatapath` parameter followed by the name of your mod's folder.

Launch Parameters can be set in many different ways and I will write this section later.

At this point your mod will do nothing because we haven't added anything to it. So let's fix that.

## Setting up the Campaign

