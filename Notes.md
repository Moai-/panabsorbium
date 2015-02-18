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

Now, let's add some armour.

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

I changed the name of the armour, the description, all file references (like `steel_mail.png` to `super_mail.png`), and gave it a little boost in armour rating, up to 15. That's all we have to do here for now.

**To be continued...**
