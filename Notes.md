So, we have a problem. Our stockpiles are overflowing with silver and gold. We can't sell them, we can't make things out of them, and we can't throw them at goblins. "These piles of precious metals have got to go," uttered no one until Stonehearth came along. Because we don't have a scientific education or imagination, we'll go ahead and make a suit of armour out of them.

### Tools and Setup

I use the following for my Stonehearth modding:
  * [Lua Unminifier](http://discourse.stonehearth.net/t/lua-unminifier-formatter-improved/8217)
  * [StoneVox](http://discourse.stonehearth.net/t/stonevox-3d-community-voxel-modeler-for-stonehearth-v-0-0-6/8664)
  * [Notepad++](http://notepad-plus-plus.org/)
  * [Microworld mod](https://github.com/stonehearth/microworld)

First, I make a `mods_bup` directory inside Stonehearth's root directory (`Steam\steamapps\common\Stonehearth\mods` for me, as I use the Steam version), into which I copy (backup) the contents of the `mods` folder. We'll be messing with Stonehearth's data, so we want to keep the originals handy in case if we explode something.

![Don't look at my tautological folder structure *hides*](http://puu.sh/g2vbL/00174bb1e7.png)

Next, I open up `stonehearth.smod` inside the `mods` directory, extract it, and delete `stonehearth.smod` so that SH is forced to read the unpacked version. I do this with all `.smod` files in the `mods` directory. `.smod` files are renamed zip files, and can be opened by programs like WinZip, [WinRAR](http://www.rarlab.com/download.htm), and so on. 

![I'm using TortoiseGit, if you're wondering about the icons](http://puu.sh/g2vyT/075a151593.png)

We do this in order to be able to quickly modify Stonehearth and then test our modifications in the same breath. Otherwise we'd have to unzip the smod, make our changes, and zip it back up. Stonehearth reads both `.smod` files and simple directories, so we use that to our advantage.

Finally, we launch Stonehearth with the Microworld mod. To do this, I navigate to my root Stonehearth directory, and open a command window there. I type in the following incantation to summon up an instance of our Microworld:

![Stonehearth.exe --game.main_mod=microworld](http://puu.sh/g2wq9/0ff06615c3.jpg)

Ta-da!

![Our tiny rock in the middle of eternity](http://puu.sh/g2wzx/efa604da07.jpg)

If you've opened it, great. Now close it. Let's add some armour.

### Adding armour

As a modder, Ctrl+C is my shield, Ctrl+V is my sword. We'll be using these quite a lot here, and thankfully modding Stonehearth is pretty much limited to that until you want to do some really serious stuff with Lua (and even then, I imagine, those two will be your duct tape and WD-40 a great deal of time). First, let's take a look as to what existing armour looks like.

Using Lua Unminifier, open `stonehearth.smod` inside your `mods_bup` directory. As far as I know, Lua Unminifier doesn't write to files inside `.smod`s, so we're safe using it just as a good way to navigate the game's files. 

![It should look like this](http://puu.sh/g2xsr/cdd41929d8.jpg)

Take a minute to open up Stonehearth and browse through its file directory. Many things are in directories that make sense, such as all of our armours being inside `entities/armor`, all blacksmith recipes being in `jobs/blacksmith/recipes`, and so on. So let's pop open `steel_mail`:

![The atoms of a Stonehearth steel mail](http://puu.sh/g2xYT/bde3c5a0bb.png)

Stonehearth defines entities -- "things", like trees, clothing, resources, sheep, Hearthlings, etc -- inside `.json` files. All the information for a particular entity that Stonehearth needs is either located or referenced inside that main `.json` file. What does a `.json` file for `steel_mail` look like, you ask? Well, you'll have to open the file and look yourself, if you're so curious. I'm here to add armour! I imagine my armour will be no less strong than steel mail, so I'll give my mighty Copy/Paste sword a swing:

![Whoosh](http://puu.sh/g2yuJ/548ff02d48.png)

Well... okay. We don't want it to be JUST like steel mail, we want it to be *better*. Let's rename it to something.

![Good enough](http://puu.sh/g2yKt/29a67597ea.png)

![Apply the same logic to the files inside the directory](http://puu.sh/g2yXp/57c2375f7b.png)

Since we've already established that the `.json` file is the heart of our object, let's take a look at it.

![Stuff below the cutoff isn't implemented as per this tutorial](http://puu.sh/g2z9P/9cb8c2dc11.png)

This is the file that's describing our `super_mail` armour. We see that it's in the `armor` category (oh, and pardon my Canadian spelling /late), that it's called a *Full Platemail* ingame, it's got a description, the icon it uses in game, what `.qb` objects render it in 3d, what material it's made of, what it looks like before a Hearthling puts it on, how exactly Stonehearth should put it on the Hearthling, how much armour rating it actually provides our Hearthling, and how much it's worth in gold. Wow, that was a mouthful, wasn't it? Let's change up some of these stats.

![Changed stats](http://puu.sh/g2zCF/477c3bf484.png)

I changed the name of the armour, the description, all file references (like `steel_mail.png` to `super_mail.png`), and gave it a little boost in armour rating, up to 15. That's all we have to do here for now. While we're on this `.json` spree, let's edit `super_mail_iconic.json` accordingly, just filling in the old bits of steel and replacing them with our new dose of SUPER:

![SUPER!](http://puu.sh/g2Gtl/3ad1f94e92.png)

While we already described `super_mail` the **object**, we needed to describe it as `super_mail` the **iconic object**, which is the little minified version of the armour that you see in the stockpile before your guys equip it.

Let's save and close out, then open our StoneVox editor. Make sure not to click anything while it's loading: I found that if the main window doesn't have focus, sometimes it complains and hangs up. Once it's loaded, drag and drop the `super_mail.qb` file into the main StoneVox window.

![Our mail](http://puu.sh/g2Aub/b37f6e0ea1.jpg)

Let's add some modifications to it. Feel free to be as creative as you like. I have the artistic talent of a mindless null, so I'll just recolour some bits on the main armour:

![I'm so sorry, Radiant](http://puu.sh/g2ARh/61fdfca0fb.jpg)

Some tips: use the paintbrush icon to paint voxels, use the pencil to add voxels to the model. Don't forget to change matrices -- think "moving parts" or "components" of a model -- when you want to edit different parts of the body. The `<<` button in the middle right opens up the matrix menu. Hold right mouse to rotate around the model, scroll through the scrollwheel to zoom in or out, and hold middle mouse to pan in any direction. When you're done, take a snapshot of your armour using the photo camera icon on the lower right (we'll use this for our in-game icon of your armour), then press the floppy disk icon in the lower middle-right to export the file as a `.qb` file. You will find both the `.png` file and the `.qb` file in the directory where you put StoneVox:

![There's Waldo](http://puu.sh/g2BSk/3db64b4611.jpg)

Grab them from that folder, and put them back into `stonehearth/entities/armor/super_mail`. Overwrite when promped. If you're still feeling inspired from editing that platemail, feel free to also edit the `iconic` file and the `female` variant.

Okay. So now we've got our `super_mail`, the item. The only thing we need to do now is to tell Stonehearth that it exists. Stonehearth's main reference for all the entities it has access to is the `manifest.json` file. It associates in-code references with on-disk file assets, such as everything we've created for our Super Mail thus far. Clearly, it had to have associated steel mail with its resources at some point. We remember that steel mail was referred to in code as `steel_mail`. So let's open up `manifest.json` and do a search for `steel_mail`:

![Thoughts of armour](http://puu.sh/g2CvQ/dcec0a3bff.png)

Among other things, we see how `steel_mail` was exposed to Stonehearth. Let's plagiarize! A copy here, a paste there, a fix hither, a hack yonder, and we end up with:

![IT CAN SEE US NOW](http://puu.sh/g2CK5/c6ca377e0c.png)

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
