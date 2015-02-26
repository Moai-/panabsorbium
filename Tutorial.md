# Do It Yourself

Stonehearth was designed to be a mod-friendly game. Everything we do here can be accomplished with nothing more than renaming and copy-pasting certain things. If you are afraid to start because you don't know how to code, don't let that stop you. Most of what we'll be doing here is re-naming and copy-pasting configuration files rather than code.

As of this writing (February 2015), Stonehearth is in Alpha 8. I will try to keep this tutorial up to date as new updates roll out and key files undergo change. As such, I will keep this tutorial simple and limited in scope, large enough to get you started on modding, and small enough to be updated easily. Our objectives are as follow:
 * Add new armor to the game
 * Make the new armor craftable in-game
 * Add a new ingot to craft your armor with

It's Alpha 8 and one can hardly kick a rock on a mountain without tripping on a glistening chunk of gold ore. So, we have a problem. Our stockpiles are overflowing with silver and gold. We can't sell them, we can't make things out of them, and we can't throw them at goblins. "These piles of precious metals have got to go," uttered no one until Stonehearth entered Alpha. For some reason, we decide that the best use for surplus bullion is making it into a suit of armor.

### Compatibility
Compatible as of **[dev2193]**  
Last updated on **February 26, 2015**  
View finished mod at https://github.com/Moai-/panabsorbium/

### Tools and Setup

I use the following for my Stonehearth modding:
  * [Lua Unminifier](http://discourse.stonehearth.net/t/lua-unminifier-formatter-improved/8217)
  * [StoneVox](http://discourse.stonehearth.net/t/stonevox-3d-community-voxel-modeler-for-stonehearth-v-0-0-6/8664)
  * [Notepad++](http://notepad-plus-plus.org/)
  * [Microworld mod](https://github.com/stonehearth/microworld)
 
If you already have these tools, or if you're using something else (e.g. Qubicle Constructor instead of StoneVox, SublimeText instead of Notepad++ and Lua Unminifier), feel free to skip the following section. Keep in mind that this tutorial assumes you're using the setup mentioned above/below.

I like to keep my tools close to where I use them, so I create a folder called "tools" in my Stonehearth root directory (`Steam\steamapps\common\Stonehearth` for me, as I use the Steam version -- this is what I mean whenever I refer to "Stonehearth root directory"). I extract the Lua Unminifier and StoneVox programs into it:

![Thusly](http://i.imgur.com/jNUxF5X.png)

Yours won't look exactly like mine, but the idea should be similar enough. To run StoneVox, I run `run.bat` in its folder; to run Lua Unminifier, I run `LuaUnminifier.exe` in its folder.

 Once I have `microworld.smod`, I drop it into my `mods` folder. Notepad++ is fairly easy to install on Windows.

StoneVox is an open-source `.qb` editor, which we will use to modify in-game assets. You can, of course, also create new ones from scratch, but I have the artistic gift of a mindless null and the heart of a modder, so I let my Ctrl+C and Ctrl+V do the talking. 

LuaUnminifier is not essential for this mod, but I use it anyway for its capability to render minified Lua in a readable format, and for its convenient "search entire project" function. Although coding skills aren't essential for modding in general or this specific tutorial, the ability to debug an error will always come in handy when working with computers. Stonehearth is good about providing informative errors, so if you see that the game is complaining about not being able to find `some_item`, you do a search for `some_item` in the project, and realize you forgot to add `some_item.json` to the folder where it was supposed to be.

Ok, got all the tools? Great. **If you skipped the previous part, begin again here.**

First, I make a `mods_bup` directory inside Stonehearth's root directory, into which I copy (backup) the contents of the `mods` folder. We'll be messing with Stonehearth's data, so we want to keep the originals handy in case if we explode something.

![Don't look at my tautological folder structure *hides*](http://i.imgur.com/C8BPodN.png)

Next, I open up `stonehearth.smod` inside the `mods` directory, extract it, and delete `stonehearth.smod` so that SH is forced to read the unpacked version. These are the files we're going to be playing with. I do this with all `.smod` files in the `mods` directory. `.smod` files are renamed zip files, and can be opened by programs like WinZip, [WinRAR](http://www.rarlab.com/download.htm), and so on. 

![I'm using TortoiseGit, if you're wondering about the icons](http://i.imgur.com/eAlHG11.png)

We do this in order to be able to quickly modify Stonehearth and then test our modifications in the same breath. Otherwise we'd have to unzip the smod, make our changes, and zip it back up. Stonehearth reads both `.smod` files and simple directories, so we use that to our advantage.

*Note*: I use LuaUnminifier to browse and view my backup copy of Stonehearth's data: `Stonehearth/mods_bup/stonehearth.smod`, but whenever I modify a file, I do it through Notepad++ inside the unpacked mod folder, like: `Stonehearth/mods/stonehearth/entities/armor/leather_vest/leather_vest.json`. Yes, that does mean that the two copies will be out of sync eventually, and yes, this would be a terrible idea to do in a larger mod; however, as soon as we learn how to package our mod into a `.smod` file, it won't matter; we will no longer be modifying Stonehearth's files directly.

Finally, we launch Stonehearth with the Microworld mod. Microworld is, as the name suggests, a tiny world which is generated according to strict parameters that we can change around at will. This means that when we need to test an item that wouldn't normally be available until later in the game, we can just code it in to drop at our settlers' feet in the Microworld as the game starts. Very useful, yes? Let's start it up.

To do this, I navigate to my root Stonehearth directory, and open a command window there (shift+click anywhere in the explorer window, but not on any one file).

![Or just do Windows button+R, then cd "your stonehearth directory"](http://i.imgur.com/vjaJMhH.png)

I type in the following incantation to summon an instance of our Microworld:

![Stonehearth.exe --game.main_mod=microworld](http://i.imgur.com/COJj3Yl.png)

Ta-da!

![Our tiny rock in the middle of eternity](http://i.imgur.com/bCn0TBz.png)

If you've opened it, great. You've seen how fast it is to load, and how it skips the title page, letting you code lightning-fast. Now close it. Let's add some armor.

### Adding armor

As a modder, Ctrl+C is my shield, Ctrl+V is my sword. As I've alluded to before, we'll be using these quite a lot here, and thankfully modding Stonehearth is pretty much limited to that until you want to do some really serious stuff with Lua. Even then, I imagine, those two will be your duct tape and WD-40 a great deal of time. To get some background, let's take a look as to what existing armor looks like.

Using Lua Unminifier, open `stonehearth.smod` inside your `mods_bup` directory.

![It should look like this](http://i.imgur.com/5aztqWf.png)

Take a minute to explore Stonehearth and browse through its file directory. Many things are in directories that make sense, such as all of our armors being inside `entities/armor`, all blacksmith recipes being in `jobs/blacksmith/recipes`, and so on. It's like the Matrix, or staring at the wall while in the washroom: look long enough and patterns start to show up. You'll see labelled values, numbers, assets, etc that correspond to in-game objects. Changing them here changes them in-game, through dark magic. 

We're making an armor, and we need a place to start. So let's pop open `steel_mail`:

![The atoms of a Stonehearth steel mail](http://i.imgur.com/chVBZsd.png)

Stonehearth defines entities -- "things", like trees, clothing, resources, sheep, Hearthlings, etc -- inside `.json` files. All the information for a particular entity that Stonehearth needs is either located or referenced inside that main `.json` file with the same name as the entity: things like its in-game name, what picture it uses to represent itself, what model it's rendered as in the world, what effects it has, and so on. What does a `.json` file for `steel_mail` look like, you ask? Well, you'll have to open the file and look yourself, if you're so curious. I'm here to add armor! I switch to my `mods` directory, so I can edit it. I imagine my armor will be no less strong than steel mail, so I'll give my mighty Copy/Paste sword a swing:

![Whoosh](http://i.imgur.com/2eRReno.png)

Well... okay. We don't want it to be JUST like steel mail, we want it to be *better*. Let's rename it to something.

![Good enough](http://i.imgur.com/b5v9G6i.png)

And thus, Panabsorbium makes its entrance. A strange material that's cold to the touch, it's made of heavy precious metals and is somehow lighter and stronger than both / either of them. Good enough.

On the inside, we've still got files that have `steel_mail` in their name. Let's change that to our new name as well. Protip: copy+paste works well here, too.

![Apply the same logic to the files inside the directory](http://i.imgur.com/MlZe5XA.png)

Since we've already established that the `.json` file is the heart of our object, as far as Stonehearth is concerned, let's take a look at it.

    {
       "type": "entity",
       "mixins" : "stonehearth:mixins:item_properties",
       "components" : {
          "item" : {
             "category" : "armor"   
          },
          "unit_info" : {
             "name": "Full Platemail",
             "description": "A full suit of steel armor",
             "icon" : "file(steel_mail.png)"         
          },
          "model_variants": {
             "default": {
                "layer": "clothing",
                "models": [
                   "file(steel_mail.qb)"
                ]
             },
             "female" : {
                "layer": "clothing",
                "models": [
                   "file(steel_mail_female.qb)"
                ]
             }
          },
          "stonehearth:material" : {
             "tags" : "steel armor heavy_armor"
          },
          "stonehearth:entity_forms" : {
             "iconic_form" : "file(steel_mail_iconic.json)"
          },
          "stonehearth:equipment_piece" : {
             "render_type" : "merge_with_model",
             "slot" : "torso",
             "ilevel" : 9,
             "roles" : "combat",
             "equip_effect" : "/stonehearth/data/effects/level_up"
          }
       },
       "entity_data" : {
          "stonehearth:combat:armor_data" : {
             "base_damage_reduction" : 9
          },
          "stonehearth:net_worth" : {
             "value_in_gold" : 350,
             "rarity" : "common",
             "shop_info" : {
                "buyable" : true,   
                "sellable" : true,
                "shopkeeper_level" : 1,
                "shopkeeper_type" : "caravan"
             }
          }
       }   
    }

Well, it may look like a lot, but it really isn't. When you see something in the code that you don't know about, leave it alone, and only poke it if it starts causing problems (or if you've got a safe backup). Otherwise, your poking will be the very thing that causes those problems. Having said that, here are some of the fields that might interest us:

*Note*: if you're unfamiliar with JSON, `unit_info.name` is referring to the `name` property inside the `unit_info`'s braces.

 * `unit_info.name`: The in-game name of this entity.
 * `unit_info.description`: The description seen on the entity.
 * `unit_info.icon`: A PNG file representing the entity in menus and the like.
 * `model_variants`: Here, we link the QB files to the default (male) and female armor variants.
 * `stonehearth:entity_forms.iconic_form`: This is the QB file that will represent the iconic form of your armor.*
 * `entity_data.stonehearth:combat:armor_data.base_damage_reduction`: What a mouthful, thankfully pretty self-explanatory. It determines how effective your armor is at stopping damage dealt to the Hearthling.
 
*Think stockpiles: a piece of armor looks way different when it's stored in a stockpile (or carried by a Hearthling as an item) versus when it's being worn by a Hearthling. A piece of furniture looks different if it's in a stockpile/being carried, versus when it's placed. As far as Stonehearth is concerned, these are separate objects! They turn into one another depending on the in-game circumstances, like "I was worn" or "I was undeployed".

Let's change some of these around. You've probably noticed file references that look like this: `"icon" : "file(steel_mail.png)"`. This refers to the file called `steel_mail.png` in the same directory as this `.json` file. We have no such thing in this folder; we have one called `panabsorbium_vest.png` now. Let's change this and all other file references accordingly:

    "icon" : "file(panabsorbium_vest.png)"
    "models": [
       "file(panabsorbium_vest.qb)"
    ]
    "models": [
       "file(panabsorbium_vest_female.qb)"
    ]
    "iconic_form": "file(panabsorbium_vest_iconic.json"

Good. Now, the fun parts:

    "name": "Panabsorbium Vest"
    "description": "It's cold to the touch"
    "base_damage_reduction": 15

The JSON-ish "heart" of the armor is now configured. The armor is now cheesy, and stops lots of damage. Remember, though, as was mentioned before, that many objects, including armors, have two forms. One is when the object is worn, and the other is a smaller, less detailed version that's shown when it's on the ground, in a stockpile, or being carried by a Hearthling. This is called the "iconic" form, and it's considered a separate, though related, object by Stonehearth. It also comes with its own JSON file, the renamed `panabsorbium_vest_iconic.json`. Let's pop it open.

    {
       "mixins": "stonehearth:mixins:item_properties",
       "type": "entity",
       "components": {
          "unit_info" : {
             "name": "Full Platemail",
             "description": "A full suit of steel armor",
             "icon" : "file(steel_mail.png)"
          },
          "model_variants": {
             "default": {
                "models": [
                   "file(steel_mail_iconic.qb)"
                ]
             }
          },
          "mob" : {
             "model_origin" : { "x": -0.05, "y": 0, "z": 0.05 }
          },
          "stonehearth:material" : {
             "tags" : "steel armor heavy_armor"
          }
       }
    }

Since the iconic form can't be equipped, there are no combat stats here of any sort. Instead, we've got the name and description, which we can fill from the previous file with copy/paste, and some broken file references. We won't touch the placement information in `mob`, and I'm ignoring all `stonehearth:material` tags because I personally don't know what impact it will have on the game. Fixing the known issues, then, we get:

    {
       "mixins": "stonehearth:mixins:item_properties",
       "type": "entity",
       "components": {
          "unit_info" : {
             "name": "Panabsorbium Vest",
             "description": "It's cold to the touch",
             "icon" : "file(panabsorbium_vest.png)"
          },
          "model_variants": {
             "default": {
                "models": [
                   "file(panabsorbium_vest_iconic.qb)"
                ]
             }
          },
          "mob" : {
             "model_origin" : { "x": -0.05, "y": 0, "z": 0.05 }
          },
          "stonehearth:material" : {
             "tags" : "steel armor heavy_armor"
          }
       }
    }
 
Okay! Our JSON work is done for now. Both the regular form and the iconic form of the armor is configured. Let's see if we can make some cool looking armor out of this.

Let's save our work and close out, then open our StoneVox editor. Make sure not to click anything while it's loading: I found that if the main window doesn't have focus, sometimes it complains and hangs up. Once it's loaded, drag and drop the `panabsorbium_vest.qb` file into the main StoneVox window.

![Our mail](http://i.imgur.com/Z8hxB5J.png)

There's the old Steel Mail qb file. Although pretty, as of this writing (Stonehearth in Alpha 8), the helmet does not render on Hearthlings properly. So, rather than using the Steel Mail as a template for our armor, let's look around. Note: again, as of this writing, StoneVox gets grumpy when you try to load more than one file per program execution. Before loading more `.qb` files, close the program, then execute `run.bat` again.

After looking around, I've decided to settle on `leather_vest.qb` as a template for my male/default armor, from the `leather_vest` folder...

![Thanks Radiant](http://i.imgur.com/xKBUjk1.png)

... and `cloth_padded_vest_female.qb`, from `cloth_padded_vest`, for my female armor:

![Thanks again Radiant](http://i.imgur.com/ESmBnnk.png)

Let's not forget about the iconic file, too, which I'll steal once again from `cloth_padded_vest_iconic.qb`:

![And again](http://i.imgur.com/dgywvcH.png)

Add some modifications to each one. Feel free to be as creative as you like. 

Some tips: use the paintbrush icon to paint voxels, use the pencil to add voxels to the model. Don't forget to change matrices -- think "moving parts" or "components" of a model -- when you want to edit different parts of the body. The `<<` button in the middle right opens up the matrix menu. The iconic model doesn't have these, as it doesn't have any animated parts. Hold right mouse to rotate around the model, scroll through the scrollwheel to zoom in or out, and hold middle mouse to pan in any direction. When you're done, take a snapshot of your armor (just the male/default one) using the photo camera icon on the lower right (we'll use this for our in-game icon of your armor), then press the floppy disk icon in the lower middle-right to export the file as a `.qb` file. You will find both the `.png` file and the `.qb` file in the directory where you put StoneVox, inside the `export` folder:

![There's Waldo](http://i.imgur.com/OsONwVN.png)

Grab them from that folder, and put them back into `stonehearth/entities/armor/panabsorbium_vest`. For posterity, here's what I ended up with:

![Male, female, and iconic, from left to right](http://i.imgur.com/RluUbCY.png)

And the icon...

![Red and white, boros represent](http://i.imgur.com/HTua2kR.png)

Okay. So now we've got our `panabsorbium_vest`, the item. The only thing we need to do now is to tell Stonehearth that it exists. Stonehearth's main reference for all the entities it has access to is the `manifest.json` file, found in `Stonehearth/mods/stonehearth`. It associates in-code references with on-disk file assets, such as everything we've created for our Panabsorbium Vest thus far. Since steel mail works as an armor, `manifest.json` had to have associated steel mail with its resources at some point. We remember that steel mail was referred to in code as `steel_mail`. So let's open up `manifest.json` and do a search for `steel_mail`:

    "armor:iron_mail" : "file(entities/armor/iron_mail)",
    "armor:steel_mail" : "file(entities/armor/steel_mail)",
    "armor:wooden_shield" : "file(entities/armor/wooden_shield)",
    "armor:basic_shield" : "file(entities/armor/basic_shield)",

Among other things, we see how `steel_mail` was exposed to Stonehearth. Let's plagiarize! A copy here, a paste there, a fix hither, a hack yonder, and we end up with a new line:

    "armor:iron_mail" : "file(entities/armor/iron_mail)",
    "armor:steel_mail" : "file(entities/armor/steel_mail)",
    "armor:panabsorbium_vest" : "file(entities/armor/panabsorbium_vest)",
    "armor:wooden_shield" : "file(entities/armor/wooden_shield)",
    "armor:basic_shield" : "file(entities/armor/basic_shield)",

Great. On paper, at least. What's the motivation for doing all this if you can't see it in-game? Great question, let's solve it. We haven't put this armor as a recipe for the blacksmith yet, so there's no in-game way we can get it. However, for testing purposes, we can cheat it into the game by generating a world right with it!

### Testing the armor

Let's open up `mods/microworld/worlds/mini_game_world.lua` -- the file that's responsible for populating our microworld whenever we launch it -- and look near the bottom:

    pickup(workers[6], 'stonehearth:resources:wood:oak_log')
    pickup(workers[2], 'stonehearth:resources:fiber:silkweed_bundle')
    pickup(workers[3], 'stonehearth:trapper:talisman')
    pickup(workers[4], 'stonehearth:carpenter:talisman')
 
Interesting. Code that's apparently referencing items to give to our workers, and *somehow*, whenever we launch the Microworld, *those workers start with those exact items!* It must be a lucky coincidence! Let's exploit it. We want to give our workers the armor and a wooden sword to get promoted with quickly:

    pickup(workers[6], 'stonehearth:armor:panabsorbium_armor')
    pickup(workers[2], 'stonehearth:footman:wooden_sword_talisman')
    pickup(workers[3], 'stonehearth:trapper:talisman')
    pickup(workers[4], 'stonehearth:carpenter:talisman')

Remember how we declared our armor in `stonehearth/manifest.json`? This means that the package called `panabsorbium_armor` belongs to the mod called `stonehearth`. We treat it as such in this lua file as well, hence the `stonehearth:` in front.

(TODO: Make it drop the iconic version rather than full-sized)

Now go back to the terminal where we opened Microworldo, and press the Up Arrow key to re-type the secret incantation. Once inside the Microworld, create a stockpile, promote a Hearthling to a footman. After the armor gets dropped (and turns into its iconic version), then placed into the stockpile, watch your footman equip the suit of armor:

![Can you see the greenness of envy in Leah's eyes as she gazes upon Orsa's new Panabsorbium bling?](http://i.imgur.com/pguh9Tg.png)

Yaaay!

... but wait. We're not going to cheat our armor into the game every time. It's time to think how we'll be...

### Making the armor craftable

Pop quiz. Who usually makes armor? No, it's not the worker; no, it's not the shepherd; and no, it's not the cute red fox. You fail Stonehearth. It's actually the Blacksmith. The weaver also makes some, but our armor is going to be based on metals rather than on cute red foxes, so we'll make our Blacksmith make it.

First, some investigation reveals the existence of a `/jobs/blacksmith`:

![I wonder...](http://i.imgur.com/oQoZqVy.png)

That "recipes" folder is looking awfully alluring for what we're trying to do. Let's take a look-see inside.

![Recipes!](http://i.imgur.com/GVYwfDd.png)

Predictably, here are all of the Blacksmith's recipes. There are two things we'll have to do here; first, we tell the Blacksmith that he can craft our Panabsorbium Vest, and then, we tell him how he can pull it off. Let's take a look at `recipes.json`, where the Blacksmith keeps track of *what* he can do.

Among the other recipes, we see the section that interests us, the armor:

    "armor" : {
       "ordinal" : 12, 
       "name" : "Armor",
       "recipes" : {
          "bronze_breastplate" : {
             "recipe" : "file(bronze_breastplate_recipe.json)"
          },
          "iron_mail" : {
             "recipe" : "file(iron_mail_recipe.json)"
          },
          "steel_mail" : {
             "recipe" : "file(steel_mail_recipe.json)"
          },
          "basic_shield" : {
             "recipe" : "file(basic_shield_recipe.json)"
          }            
       }
    }

We use the ancient modders' technique:

    "armor" : {
       "ordinal" : 12, 
       "name" : "Armor",
       "recipes" : {
          "bronze_breastplate" : {
             "recipe" : "file(bronze_breastplate_recipe.json)"
          },
          "iron_mail" : {
             "recipe" : "file(iron_mail_recipe.json)"
          },
          "steel_mail" : {
             "recipe" : "file(steel_mail_recipe.json)"
          },
          "steel_mail" : {
             "recipe" : "file(steel_mail_recipe.json)"
          },
          "basic_shield" : {
             "recipe" : "file(basic_shield_recipe.json)"
          }            
       }
    }
 
...

    "armor" : {
       "ordinal" : 12, 
       "name" : "Armor",
       "recipes" : {
          "bronze_breastplate" : {
             "recipe" : "file(bronze_breastplate_recipe.json)"
          },
          "iron_mail" : {
             "recipe" : "file(iron_mail_recipe.json)"
          },
          "panabsorbium_vest" : {
            "recipe" : "file(panabsorbium_vest_recipe.json)"
          },
          "steel_mail" : {
             "recipe" : "file(steel_mail_recipe.json)"
          },
          "basic_shield" : {
             "recipe" : "file(basic_shield_recipe.json)"
          }            
       }
    }

*Note*: if your game starts exploding because you can't open a JSON file, try copy+pasting the problematic file's contents  [here](http://jsonlint.com/) and seeing if it comes up with any issues. 

The Blacksmith now knows that there's this thing called a `panabsorbium_vest` that he can apparently craft, but he doesn't actually have a clue how he can do it. He has all these other recipes, but `panabsorbium_vest_recipe.json` doesn't exist, thus he calls us liars and errors out. Let's create it, using our favourite modding trick!

![here we go again](http://i.imgur.com/uXBzE7n.png)
![and here we are](http://i.imgur.com/XyOfwaH.png)

Here's where we tell the Blacksmith *how* he can make the vest: what ingredients go in, how long it will take him, and what should come out the other end.

    {
       "type":"recipe",
      
       "work_units"  : 5,
       "recipe_name" : "Full Platemail",
       "description" : "A full suit of heavy, durable steel armor.",
       "flavor" : "Full Metal Jacket",
       "portrait"    : "/stonehearth/entities/armor/steel_mail/steel_mail.png",
       "level_requirement" : 4,
       
       "ingredients": [
          {
             "uri" : "stonehearth:refined:steel_ingot",
             "count" : 4
          },
          {
             "material" : "leather resource",
             "count" : 1
          },
          {
             "material" : "thread resource",
             "count" : 1
          }
      ],
      "produces": [
          {
             "item":"stonehearth:armor:steel_mail"
          }
      ]
    }

Most of this should seem familiar to you. This is an entity just like our armor that we made previously. It may not have a "physical" form, like the armor did in the shape of `.qb` files, but it's still very much an entity that may be displayed in the game's various menus. Thus, the two old fields `name` and `description` pop up, which can be safely copy-pasted into. Some items worth going over:

 * `work_units`: This is the number of animations the Hearthling will make at his or her workbench after gathering all required items and before removing the finished item. It would follow that small tasks like ingots take only a couple work units, while larger tasks like creating a physically impossible armor might take more: say, 8.
 * `flavour`: Not all items have a flavour text, but it seems that many which don't are nevertheless capable of having it, as we'll see later with the ingot. I filled this bit in for fun, but you don't have to. Anyway, to avoid confusion, I would recommend putting the same name and description for the item across all instances of the same item.
 * `level_requirement`: For this armor, I'd set this to 5. However, since getting a Blacksmith to level 5 in a quick game is difficult, we instead remove the level requirement while we're testing. This means the armor can be made by a freshly promoted novice.
 
Here's the finished recipe. Let's make it work with just existing bars for now, and only use one of each for testing. Once we've got our blacksmith smithing the thing, we can change the requirements.


    {
       "type":"recipe",
       "work_units"  : 8,
       "recipe_name" : "Panabsorbium Vest",
       "description" : "It's cold to the touch",
       "flavor" : "Strong but light armor.",
       "portrait"    : "/stonehearth/entities/armor/panabsorbium_vest/panabsorbium_vest.png",
       "ingredients": [
          {
             "uri" : "stonehearth:refined:silver_ingot",
             "count" : 1
          },
          {
            "uri" : "stonehearth:refined:gold_ingot",
            "count" : 1
          }
       ],
       "produces": [
          {
             "item":"stonehearth:armor:panabsorbium_vest"
          }
       ]
    }

Looks good! Now let's test it.

Give our workers a Blacksmith's Hammer, the two bars, a rock (to build the workshop), and a wooden sword. Remember that we're asking the `stonehearth` mod, in the game's language, for some packages. All things belonging to the `stonehearth` mod are inside its `manifest.json`. Searching through the `manifest.json` file gives us the item names to put into our `mini_game_world.lua`, prefixed with the "mod" they're coming from:

    pickup(workers[6], 'stonehearth:footman:wooden_sword_talisman')
    pickup(workers[2], 'stonehearth:refined:gold_ingot')
    pickup(workers[3], 'stonehearth:refined:silver_ingot')
    pickup(workers[4], 'stonehearth:blacksmith:talisman')   
    pickup(workers[1], 'stonehearth:resources:stone:hunk_of_stone')

Start the Microworld again, promote a Hearthling to blacksmith and another to footman, have the blacksmith build the anvil and try to craft the armour. If all goes well...

![also sprach zarathustra in the background intensifies](http://i.imgur.com/3SRzehl.png)

Excellent! Now, to add that air of mystique, let's make our armour also have its very own ingot.

### Adding a custom resource

As with the case of our armor and crafting recipe for it, we will follow the pattern:

 1. Look at how it was done before
 2. Copy an existing example
 3. Modify the copy to fit our needs

Let's first find where all the other ingots are.

![Hint: they're here](http://i.imgur.com/sE0M3ru.png)

Like the Karate Kid up-down-up-down with the brush on the house to become a great fighter, so do we copy-paste, copy-paste on our way to being a great modder.

![](http://i.imgur.com/FiBI4Bi.png)
![](http://i.imgur.com/7ryPOMX.png)
![](http://i.imgur.com/F7jN9Ip.png)

We take a look inside the `.json`:

    {
       "mixins": "stonehearth:mixins:item_properties",
       "type": "entity",
       "components": {
          "item": {
             "material": "gold ingot",
             "category": "resources",
             "stacks": 40
          },
          "stonehearth:material" : {
             "tags" : "gold ingot resource"
          },
          "model_variants": {
             "default": {
                "models": [
                   "file(gold_ingot.qb)"
                ]
             }
          },
          "effect_list" : {
             "effects" : [
                "stonehearth:effects:treasure_glow"
             ]  
          },     
          "unit_info": {
             "name": "Gold Ingot",
             "description": "A bar of pure gold. A suitable material for crafted goods.",
             "icon" : "file(gold_ingot.png)"
          }
       },
       "entity_data" : {
          "stonehearth:net_worth" : {
             "value_in_gold" : 30,
             "rarity" : "common",
             "shop_info" : {
                "buyable" : true,   
                "sellable" : true,
                "shopkeeper_level" : 1,
                "shopkeeper_type" : "caravan"
             }
          }
       }
    }

Standard fare. We fix our filenames and add new names and description to things. By virtue of having copied the gold bar, we also inherit a nice sparkly glow around our new bars, just like gold bars. This time, I change the `material` and `tags` entries to "panabsorbium" to see if anything will happen. Also, I put in a flavor text, which wasn't there before on the gold bar, as I noticed that in other items, the `name` and `description` fields are accompanied by a `flavor` field. Note, also, that the item is small enough not to require an iconic form.

    {
       "mixins": "stonehearth:mixins:item_properties",
       "type": "entity",
       "components": {
          "item": {
             "material": "panabsorbium ingot",
             "category": "resources",
             "stacks": 40
          },
          "stonehearth:material" : {
             "tags" : "panabsorbium ingot resource"
          },
          "model_variants": {
             "default": {
                "models": [
                   "file(panabsorbium_ingot.qb)"
                ]
             }
          },
          "effect_list" : {
             "effects" : [
                "stonehearth:effects:treasure_glow"
             ]  
          },     
          "unit_info": {
             "name": "panabsorbium Ingot",
             "description": "A bar of pure Panabsorbium.",
             "flavor": "A show of Blacksmithing skill.",
             "icon" : "file(panabsorbium_ingot.png)"
          }
       },
       "entity_data" : {
          "stonehearth:net_worth" : {
             "value_in_gold" : 30,
             "rarity" : "common",
             "shop_info" : {
                "buyable" : true,   
                "sellable" : true,
                "shopkeeper_level" : 1,
                "shopkeeper_type" : "caravan"
             }
          }
       }
    }

Excellent. Now you can use StoneVox or QC to edit the resource bar `.qb` file, and take a new inventory photo of it as well. When you're done, drop your new files into the `panabsorbium_ingot` directory, overwriting if necessary.

![](http://i.imgur.com/YRzUmQl.png)
![](http://i.imgur.com/fGpyaaY.png)

And we remember to tell Stonehearth that the bar exists, in `manifest.json`:

    "refined:iron_ingot" : "file(entities/refined/iron_ingot)",
    "refined:gold_ingot" : "file(entities/refined/gold_ingot)",
    "refined:silver_ingot" : "file(entities/refined/silver_ingot)",
    "refined:steel_ingot" : "file(entities/refined/steel_ingot)",
    

    "refined:iron_ingot" : "file(entities/refined/iron_ingot)",
    "refined:gold_ingot" : "file(entities/refined/gold_ingot)",
    "refined:panabsorbium_ingot" : "file(entities/refined/panabsorbium_ingot)",
    "refined:silver_ingot" : "file(entities/refined/silver_ingot)",
    "refined:steel_ingot" : "file(entities/refined/steel_ingot)",

Awesome. Let's now change the smithing requirement of our vest to take in one `panabsorbium_ingot`, in `jobs/blacksmith/recipes/panabsorbium_vest_recipe.json`:

    {
       "type":"recipe",
       "work_units"  : 5,
       "recipe_name" : "Panabsorbium Vest",
       "description" : "Strong but light armour.",
       "flavor" : "It's cold to the touch.",
       "portrait"    : "/stonehearth/entities/armor/panabsorbium_vest/panabsorbium_vest.png",
       "ingredients": [
          {
             "uri" : "stonehearth:refined:panabsorbium_ingot",
             "count" : 1
          },
      ],
      "produces": [
          {
             "item":"stonehearth:armor:panabsorbium_vest"
          }
      ]
    }
 
Then we tell the Blacksmith that he can now craft this ingot inside `jobs/blacksmith/recipes/recipes.json`:

    "gold_ingot" : {
       "recipe" : "file(gold_ingot_recipe.json)"
    },
    "panabsorbium_ingot" : {
       "recipe" : "file(panabsorbium_ingot_recipe.json)"
    },
    "steel_ingot" : {
       "recipe" : "file(steel_ingot_recipe.json)"
    }

and give him the recipe by making `panabsorbium_ingot_recipe.json` based off a copy of `gold_ingot_recipe.json`:

    {
       "type":"recipe",
       "work_units"  : 2,
       "recipe_name" : "Panabsorbium Ingot",
       "description": "A bar of pure panabsorbium.",
       "flavor"      : "A",
       "portrait"    : "/stonehearth/entities/refined/panabsorbium_ingot/panabsorbium_ingot.png",
       "level_requirement" : 0,
       "ingredients": [
          {
             "uri" : "stonehearth:refined:gold_ingot",
             "count" : 1
          },
          {
             "uri" : "stonehearth:refined:silver_ingot",
             "count" : 1
          },
          {
             "material" : "wood resource",
             "count" : 1
       ],
      "produces": [
          {
             "item" : "stonehearth:refined:panabsorbium_ingot"
          }
      ]
    }

A couple things of note here. When we're specifying an ingredient, we can do it one of two ways. Either we ask for a generic "resource", like "wood resource", or "thread resource", or we ask for a very specific item. In the first case, we use `material` to specify what the ingredient is; in the second, we use `uri`.

Make sure your miniworld starts with the materials you need:

    pickup(workers[6], 'stonehearth:footman:wooden_sword_talisman')
    pickup(workers[2], 'stonehearth:refined:gold_ingot')
    pickup(workers[3], 'stonehearth:refined:silver_ingot')
    pickup(workers[4], 'stonehearth:blacksmith:talisman')
    pickup(workers[1], 'stonehearth:resources:stone:hunk_of_stone')

Now you can go through the whole crafting chain of making a new bar, then a new item out of that bar. Check it out by starting the Microworld.

### Packaging into `.smod`

So now we have added a craftable asset into the game. We are, however, doing it a bit of a dirty way: we're directly modifying Stonehearth's code. This can have a number of undesirable effects, such as all your changes being gone with the next update, or any mods depending on Stonehearth's assets being vanilla inexplicably breaking. The right way of adding content into Stonehearth is through `.smod` files.

An `.smod` file contains all of your assets -- the `.json` files, the scripts, the images, the `.qb` files -- and a `manifest.json`, which tells Stonehearth how and where to add your content. The `.smod` itself is just a zipped-up and renamed folder.

Let's make a new folder for our mod inside `Stonehearth/mods`, and give it a name:

![Naturally](http://i.imgur.com/sZ2Agev.png)

Inside, create a file called `manifest.json`, and edit it to this:

    {
      "info" : {
        "name" : "Panabsorbium",
        "version" : 1
      },
    }
 
This is your basic mod declaration, which serves much the same purpose as Stonehearth's main `manifest.json`. It says that everything specified inside the folder containing this manifest belongs to the mod Panabsorbium. I'm not sure what the `version` field does, but it is required for the mod to work. For now, just save this file, and keep it open.

Next, we'll have to drop our assets into this folder, too. Personally, I like to recreate the folder structure inside Stonehearth for consistency, so I create three directories:

`entities/armor/panabsorbium_vest`  
`entities/refined/panabsorbium_ingot`  
`jobs/blacksmith/recipes`  

These are the four places where we've been either adding or modifying things (not including `manifest.json`, since we have our own one now) inside the main `mods/stonehearth` directory. To leave the main mod clean, let's **cut** the contents of those folders from `mods/stonehearth`, and replace them into `mods/panabsorbium`:

    entities/armor/panabsorbium_vest
      panabsorbium_vest.json
      panabsorbium_vest.png
      panabsorbium_vest.qb
      panabsorbium_vest_female.qb
      panabsorbium_vest_iconic.json
      panabsorbium_vest_iconic.qb
    entities/refined/panabsorbium_ingot
      panabsorbium_ingot.json
      panabsorbium_ingot.png
      panabsorbium_ingot.qb
    jobs/blacksmith/recipes
      panabsorbium_ingot_recipe.json
      panabsorbium_vest_recipe.json

Now we tell `manifest.json` that these things exist in our mod, and what their aliases are, like we did for Stonehearth's main `manifest.json`:

    {
      "info" : {
        "name" : "Panabsorbium",
        "version" : 1
      },
      "aliases" : {
        "armor:panabsorbium_vest" : "file(entities/armor/panabsorbium_vest)",
        "refined:panabsorbium_ingot" : "file(entities/refined/panabsorbium_ingot)",
        "buffs:panabsorbium_rush" : "file(data/buffs/panabsorbium_rush/panabsorbium_rush_buff.json)"
      }
    }

Note that there's still one change missing: the Blacksmith doesn't know that he should have new recipes! We've modified his `recipes.json` file in Stonehearth's mod directory before, but we haven't carried this change over to our new mod. Have his old `recipes.json` file open, but also create a new `recipes.json` file inside `mods/panabsorbium/jobs/blacksmith/recipes`.

We're going to mixinto this change into the old `recipes.json`. I like to think of mixintos almost as zippers: you put two files and "zip" them together, you get one file that's a combination of the two old files, with all the little teeth/data matching up perfectly. Case in point, our new `recipes.json` should look like this:

    {
      "craftable_recipes" : {
        "smelt" : {
          "recipes" : {
            "panabsorbium_ingot" : {
              "recipe" : "file(panabsorbium_ingot_recipe.json)"
            }
          }
        },
        "armor" : {
          "recipes" :{
            "panabsorbium_vest" : {
              "recipe" : "file(panabsorbium_vest_recipe.json)"
            }
          }
        }
      }
    }
 
 If you look at Stonehearth's original `recipes.json` file, you'll see the similarities. Both start with `craftable_recipes`; they both have "categories" `smelt` and `armor`, which further both have `recipes` fields; after that, we add the same lines we added to the old `recipes.json` to make our recipes work. When the two files are "zipped together", you'll have our `craftable_recipes` go into their `craftable_recipes`; our `smelt` going into their `smelt`, and so on. Stonehearth will then consider our `recipes.json` and their `recipes.json` as essentially one and the same resource.

Now we tell `manifest.json` what to mixinto:

    {
      "info" : {
        "name" : "Panabsorbium",
        "version" : 1
      },
      "aliases" : {
        "armor:panabsorbium_vest" : "file(entities/armor/panabsorbium_vest)",
        "refined:panabsorbium_ingot" : "file(entities/refined/panabsorbium_ingot)"
      },
      "mixintos" : {
        "stonehearth/jobs/blacksmith/recipes/recipes.json" : "file(jobs/blacksmith/recipes/recipes.json)"
      }
    }
 
You can leave the mod as a folder inside your `Stonehearth/mods` directory, or you can use your favourite archiving program (such as WinRAR, which I linked earlier in this post) to archive it into `panabsorbium.zip` and rename it to `panabsorbium.smod`.

When testing, remember to modify your `mini_game_world.lua`: your armor will no longer be from `stonehearth:armor:panabsorbium_vest`, as `stonehearth` is no longer the name of the mod that contains it; instead, you're going to be referring to `panabsorbium:armor:panabsorbium_vest`, as that is your new mod.

![Our freshly baked modded armor!!!](http://i.imgur.com/ZKLePn0.png)

#You're Winner!
![A winner is you](http://www.clker.com/cliparts/H/M/F/3/W/Y/trophy-yellow-cup-md.png)

### Next steps

If you got through this and still want more, try adding a buff to your new armor. Remember to look for existing examples of buffed armor, such as the advanced worker vest.
