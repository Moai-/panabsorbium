# Do It Yourself

If you want to take the long route and put this mod together yourself, you're in the right place. Some blind poking around may take place, as this tutorial roughly mirrors my own experience writing this mod.

So, we have a problem. Our stockpiles are overflowing with silver and gold. We can't sell them, we can't make things out of them, and we can't throw them at goblins. "These piles of precious metals have got to go," uttered no one until Stonehearth entered Alpha. Because we don't have a scientific education or imagination, we'll go ahead and make a suit of armour out of them.

### Tools and Setup

I use the following for my Stonehearth modding:
  * [Lua Unminifier](http://discourse.stonehearth.net/t/lua-unminifier-formatter-improved/8217)
  * [StoneVox](http://discourse.stonehearth.net/t/stonevox-3d-community-voxel-modeler-for-stonehearth-v-0-0-6/8664)
  * [Notepad++](http://notepad-plus-plus.org/)
  * [Microworld mod](https://github.com/stonehearth/microworld)
 
If you already have these tools, or if you're using something else (e.g. Qubicle Constructor instead of StoneVox, SublimeText instead of Notepad++ and Lua Unminifier), feel free to skip the following section. Keep in mind that this tutorial assumes you're using the setup mentioned above/below.

I like to keep my tools close to where I use them, so I create a folder called "tools" in my Stonehearth root directory (`Steam\steamapps\common\Stonehearth` for me, as I use the Steam version -- this is what I mean whenever I refer to "Stonehearth root directory"). I extract the Lua Unminifier and StoneVox programs into it:

![Thusly](http://i.imgur.com/jNUxF5X.png)

Once I have `microworld.smod`, I drop it into my `mods` folder. Notepad++ is fairly easy to install on Windows. Yours won't look exactly like mine, but the idea should be similar enough. To run StoneVox, I run `run.bat` in its folder; to run Lua Unminifier, I run `LuaUnminifier.exe` in its folder.

Ok, got all the tools? Great.

First, I make a `mods_bup` directory inside Stonehearth's root directory , into which I copy (backup) the contents of the `mods` folder. We'll be messing with Stonehearth's data, so we want to keep the originals handy in case if we explode something.

![Don't look at my tautological folder structure *hides*](http://i.imgur.com/C8BPodN.png)

Next, I open up `stonehearth.smod` inside the `mods` directory, extract it, and delete `stonehearth.smod` so that SH is forced to read the unpacked version. These are the files we're going to be playing with. I do this with all `.smod` files in the `mods` directory. `.smod` files are renamed zip files, and can be opened by programs like WinZip, [WinRAR](http://www.rarlab.com/download.htm), and so on. 

![I'm using TortoiseGit, if you're wondering about the icons](http://i.imgur.com/eAlHG11.png)

We do this in order to be able to quickly modify Stonehearth and then test our modifications in the same breath. Otherwise we'd have to unzip the smod, make our changes, and zip it back up. Stonehearth reads both `.smod` files and simple directories, so we use that to our advantage.

Finally, we launch Stonehearth with the Microworld mod. To do this, I navigate to my root Stonehearth directory, and open a command window there. I type in the following incantation to summon up an instance of our Microworld:

![Stonehearth.exe --game.main_mod=microworld](http://i.imgur.com/COJj3Yl.png)

Ta-da!

![Our tiny rock in the middle of eternity](http://i.imgur.com/bCn0TBz.png)

If you've opened it, great. You've seen how fast it is to load, and how it skips the title page, letting you code lightning-fast. Now close it. Let's add some armour.

### Adding armour

As a modder, Ctrl+C is my shield, Ctrl+V is my sword. We'll be using these quite a lot here, and thankfully modding Stonehearth is pretty much limited to that until you want to do some really serious stuff with Lua (and even then, I imagine, those two will be your duct tape and WD-40 a great deal of time). First, let's take a look as to what existing armour looks like.

Using Lua Unminifier, open `stonehearth.smod` inside your `mods_bup` directory. As far as I know, Lua Unminifier doesn't write to files inside `.smod`s, so we're safe using it just as a good way to navigate and search the game's files. 

![It should look like this](http://i.imgur.com/5aztqWf.png)

Take a minute to open up Stonehearth and browse through its file directory. Many things are in directories that make sense, such as all of our armours being inside `entities/armor`, all blacksmith recipes being in `jobs/blacksmith/recipes`, and so on. So let's pop open `steel_mail`:

![The atoms of a Stonehearth steel mail](http://i.imgur.com/chVBZsd.png)

Stonehearth defines entities -- "things", like trees, clothing, resources, sheep, Hearthlings, etc -- inside `.json` files. All the information for a particular entity that Stonehearth needs is either located or referenced inside that main `.json` file with the same name as the armour. What does a `.json` file for `steel_mail` look like, you ask? Well, you'll have to open the file and look yourself, if you're so curious. I'm here to add armour! I imagine my armour will be no less strong than steel mail, so I'll give my mighty Copy/Paste sword a swing:

![Whoosh](http://i.imgur.com/2eRReno.png)

Well... okay. We don't want it to be JUST like steel mail, we want it to be *better*. Let's rename it to something.

![Good enough](http://i.imgur.com/b5v9G6i.png)

And thus, Panabsorbium makes its entrance. A strange material that's cold to the touch, it's made of heavy precious metals and is somehow lighter and stronger than both / either of them. Good enough.

On the inside, we've still got files that have `steel_mail` in their name. Let's change that to our new name, too. Protip: copy+paste works well here, too.

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

Well, it may look like a lot, but it really isn't. When you see something in the code that you don't know about, leave it alone, and only poke it if it starts causing problems. Having said that, here are some of the fields that might interest us:

(note: if you're unfamiliar with JSON, `unit_info.name` is referring to the `name` property inside the `unit_info`'s braces)

 * `unit_info.name`: The in-game name of this entity.
 * `unit_info.description`: The description seen on the entity.
 * `unit_info.icon`: A PNG file representing the entity in menus and the like.
 * `model_variants`: Here, we link the QB files to the default (male) and female armour variants.
 * `stonehearth:entity_forms.iconic_form`: This is the QB file that will represent the iconic form of your armour.*
 * `entity_data.stonehearth:combat:armor_data.base_damage_reduction`: What a mouthful, thankfully pretty self-explanatory. It determines how effective your armour is at stopping damage dealt to the Hearthling.
 
*Think stockpiles: a piece of armour looks way different when it's stored in a stockpile (or carried by a Hearthling as an item) versus when it's being worn by a Hearthling. As far as Stonehearth is concerned, these are two separate objects! They turn into one another depending on the in-game circumstances.

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

The JSON-ish "heart" of the armour is now configured. The armour is now cheesy, and stops lots of damage. Remember, though, as was mentioned before, that many objects, including armours, have two forms. One is when the object is worn, and the other is a smaller, less detailed version that's shown when it's on the ground, in a stockpile, or being carried by a Hearthling. This is called the "iconic" form, and it's considered a separate, though related, object by Stonehearth. It also comes with its own JSON file, the renamed `panabsorbium_vest_iconic.json`. Let's pop it open.

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

Since the iconic form can't be equipped, there are no combat stats here of any sort. Instead, we've got the name and description, which we can fill from the previous file with copy/paste, and some broken file references. Fixing these, we get:

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
 
Okay! Our JSON work is done for now. Both the regular form and the iconic form of the armour is configured. Let's see if we can make some cool looking armour out of this.

Let's save our work and close out, then open our StoneVox editor. Make sure not to click anything while it's loading: I found that if the main window doesn't have focus, sometimes it complains and hangs up. Once it's loaded, drag and drop the `panabsorbium_vest.qb` file into the main StoneVox window.

![Our mail](http://i.imgur.com/Z8hxB5J.png)

There's the old Steel Mail qb file. Although pretty, as of this writing (Stonehearth in Alpha 9), the helmet does not render on Hearthlings properly. So, rather than using the Steel Mail as a template for our armour, let's look around. Note: again, as of this writing, StoneVox gets grumpy when you try to load more than one file per program execution. Before loading more `.qb` files, close the program, then execute `run.bat` again.

After looking around, I've decided to settle on `leather_vest.qb` as a template for my male/default armour, from the `leather_vest` folder...

![Thanks Radiant](http://i.imgur.com/xKBUjk1.png)

... and `cloth_padded_vest_female.qb`, from `cloth_padded_vest`, for my female armour:

![Thanks again Radiant](http://i.imgur.com/ESmBnnk.png)

Let's not forget about the iconic file, too, which I'll steal once again from `cloth_padded_vest_iconic.qb`:

![And again](http://i.imgur.com/dgywvcH.png)

Add some modifications to it. Feel free to be as creative as you like. 

Some tips: use the paintbrush icon to paint voxels, use the pencil to add voxels to the model. Don't forget to change matrices -- think "moving parts" or "components" of a model -- when you want to edit different parts of the body. The `<<` button in the middle right opens up the matrix menu. The iconic model doesn't have these, as it doesn't have any animated parts. Hold right mouse to rotate around the model, scroll through the scrollwheel to zoom in or out, and hold middle mouse to pan in any direction. When you're done, take a snapshot of your armour (just the male/default one) using the photo camera icon on the lower right (we'll use this for our in-game icon of your armour), then press the floppy disk icon in the lower middle-right to export the file as a `.qb` file. You will find both the `.png` file and the `.qb` file in the directory where you put StoneVox:

![There's Waldo](http://i.imgur.com/OsONwVN.png)

Grab them from that folder, and put them back into `stonehearth/entities/armor/panabsorbium_vest`. For posterity, here's what I ended up with:

![Male, female, and iconic, from left to right](http://i.imgur.com/RluUbCY.png)

And the icon...

![Red and white, boros represent](http://i.imgur.com/HTua2kR.png)

Okay. So now we've got our `panabsorbium_vest`, the item. The only thing we need to do now is to tell Stonehearth that it exists. Stonehearth's main reference for all the entities it has access to is the `manifest.json` file. It associates in-code references with on-disk file assets, such as everything we've created for our Super Mail thus far. Clearly, it had to have associated steel mail with its resources at some point. We remember that steel mail was referred to in code as `steel_mail`. So let's open up `manifest.json` and do a search for `steel_mail`:

    "armor:iron_mail" : "file(entities/armor/iron_mail)",
    "armor:steel_mail" : "file(entities/armor/steel_mail)",
    "armor:wooden_shield" : "file(entities/armor/wooden_shield)",
    "armor:basic_shield" : "file(entities/armor/basic_shield)",

Among other things, we see how `steel_mail` was exposed to Stonehearth. Let's plagiarize! A copy here, a paste there, a fix hither, a hack yonder, and we end up with:

    "armor:iron_mail" : "file(entities/armor/iron_mail)",
    "armor:steel_mail" : "file(entities/armor/steel_mail)",
    "armor:panabsorbium_vest" : "file(entities/armor/panabsorbium_vest)",
    "armor:wooden_shield" : "file(entities/armor/wooden_shield)",
    "armor:basic_shield" : "file(entities/armor/basic_shield)",

Great. On paper, at least. What's the motivation for doing all this if you can't see it in-game? Great question, let's solve it. We haven't put this armour as a recipe for the blacksmith yet, so there's no in-game way we can get it. However, for testing purposes, we can cheat it into the game by generating a world right with it!

### Testing the armour

Let's open up `mods/microworld/worlds/mini_game_world.lua` -- the file that's responsible for populating our microworld whenever we launch it -- and look near the bottom:

![Ooooh](http://puu.sh/g2F64/964d56dcac.png)

It looks like the world automatically gives our hearthlings some items. Let's give one of the hearthlings the armour that we generated, as well as a wooden sword to promote them to soldier:

![Terrible code is terrible](http://puu.sh/g2Fy8/4707c2eeba.png)

(P. S. I know I'm doing it extremely wrong, I should be giving them the iconic version, but I really don't know how to do that yet :x)

Now go back to the terminal, and press the Up Arrow key to re-type the secret incantation (I may have used it more than once):

![spamspamspam](http://puu.sh/g2FNd/e72921682e.png)

Once inside the Microworld, create a stockpile, promote a Hearthling (in my case, it has to be a male, as I've only modified the male Steel Mail) to a footman. After the armour gets dropped (and turns into its iconic version), then placed into the stockpile, watch your footman equip the suit of armour (as of this testing, helmets were malfunctioning):

![I CURSE THE GODS THAT ALLOWED ME TO EXIST](http://puu.sh/g2G22/c0d9df1bc2.jpg)

Yaaay!

### Todo:

 * Making armour craftable
 * Adding custom resource
 * Adding buff script to resource
 * Packaging into `.smod`
 * Reupload images to imgur rather than puush, for permanence
 * Let me know if you want me to write the rest
