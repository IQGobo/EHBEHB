# EHBEHB
## Else Heart.Break() Elite Hacker Board
Also known as 'Else Hacker.Board()'

Also known as 'Else Hacker.Bored()'

Or just 'Hacker Board' or 'Board' for short.

Remember the Hacker Board Hiro Protagonist uses in the Black Sun, that makes Da5id nervous? This is the implementation of said board for Else Heart.Break() ;)

By following the installation instructions below, you will gain access to a computer screen floating in space anywhere you need it, running powerfull code that will help you achieve anything.

## Disclaimer
Using the code in this project will have a massive effect on your game experience. With this, you will rule the world easily, nothing will be of any challenge anymore. It also references computers and items you may not have found yet in your playthrough, so this is definately considered a major spoiler! You will be able to get any item and reach any location with this tool. There is no spoon after all.

This is why I strongly recommend to only use this code if you have either beaten the game at least once, or you are helplessly stuck at some point.

Additionally, the Hacker Board is targeted at a tech savy audience. While it is easy to make use of the Board's basic functionality, unlocking its full potential will require some geek state of mind, nerdy nature or at least getting used to.

Code wants to be free, everyone is standing on the shoulders of giants anyways, so this code is marked as public domain. Do what you want with this, but don't get back to me complaining that it fried your hamster somehow!

## Features
The Hacker Board is the ultimate hacker's swiss army knife. You can query just about anything in the game, including, but not limited to:
- listing all characters and locations
- getting the position of any object
- travel the internet to anything you are currently investigating on the Board
- copy your findings to the clipboard or to an equipped floppy disk
- knock out any person anywhere
- relocate any object
- use an item of your choice to both summon and dismiss the Board anywhere you like

The Board features some unique and powerful registers that can be used like shell expansions:
- '@' will expand to the room you worked with recently (for example by getting an item's position)
- '?' will expand to the last message a command echoed back to you (the position information from the previous example)
- '!#' will expand to an element of your current list. E.g.: you run 'all inv my' to list the contents of your inventory. Then you use 'type !2' to get the type of the third item in your backpack, as the lists are zero indexed.
- '&' will expand to the manual backup register 'bak'. This will contain only information that you put there yourself by executing 'set bak myvalue' and can help in cases where you need to juggle two values, like in 'move [from] [to]'

Those registers will stay available even after a restart of the Board, they get saved to memory. So you can use the Board, send it away to do regular ingame stuff, then bring it up again and essentially continue where you left off, the registers are still available. The list can be loaded again with a quick 'list load' command.

You can see the contents of your registers with the 'get all' command. The current list can be shown again by issueing the 'list show' or just 'show' command.

While the Board is quite rich in features, there is most probably something missing still. For example, the audio feedback I wanted to have playing on errors or when something is saved to disk or copied to clipboard does not play as intended. And I still have to figure out a way to access the teleporter's 'SetWorldPosition()' from within the Board to skip the trip through the internet. I didn't use the hard disk feature at all and a text filter instead of just filtering by item type could be usefull as well.

### Advanced list manipulation
Lists can be quite long, so most lists will contain more text than you could see on a single screen, even this big one. That is why there is a 'pager' register that controls after how many list items the output will pause for you to press ENTER to continue. Set this to whatever value you prefer with the 'set pager #' command, replacing # with the number of items you want to see before the lists pauses. You should keep 'pager' smaller than the actual number of lines that fit on the screen, as some items have really long names that wrap several lines. This value, as any register, persists over restarts of the Hacker Board, so you don't have to reassign it every time you use the board.

Even when paging a large list, it might be too long for your taste. That's what the 'filter' register is used for: filter list contents by type of object. For example, if you are only interested in the doors of a room, do 'set filter door', then 'all room here', to get a list of all items in the current room that happen to be doors. The 'here' in the last command was a shortcut itself, it resolves to the current room instead of typing the name manually. To find other usefull filter types, use the 'type [item]' command, where [item] is either the actual name of an item, or just a register expansion again, like '!0' for the first item of your current list.

You can add and delete list items, too! To delete the second element, do 'list del !1'. Repeating this command will work as long as the list has at least two items. To add a new element, enter 'list add [text]', where [text] is anything you like. To get rid of all elements, issue 'list clear'.

Why would you do all those list transformations? Because you can keep copies of lists at positions in memory of your choice. By default, the list will be stored at memory index 1000 (well, its size is there, the elements are stored in the indexes following 1000). You can change that address globally by doing 'set lastlistaddr #', where # is the address you want the list to start at. But you can also save and load lists to and from other memory locations on the fly by addressing them as the third element after 'list save' or 'list load'. Want to keep Pixie's current inventory listing at index 20? Just do 'all inv Pixie' followed by 'list save 20'. Anytime you want to recall that list later on, just load it again with 'list load 20'.

You can also glue two lists together, this feature is called 'list join'. Now you'll understand why it would be nessessary to save lists to different locations in memory: joining two lists requires two lists to start with and you have to provide the address of the list you want to append to your current list. In the last paragraph we saved Pixie's inventory at address 20. Now we get Frank's inventory listing with 'all inv Frank'. Now we have two lists: Pixie's inventory at address 20 and Frank's inventory at the address of lastlistaddr, 1000 if you didn't change it. With 'list join 20' Pixie's inventory will be appended to your list or Frank's inventory.

Combine list manipulation with the automated command expansion and you'll get a powerfull and expressive command prompt. If for example you want to swap the modifiers in Pixie's and Frank's inventory, you could either do as above and get one inventory list, save it somewhere in memory and then get the second inventory list and join both lists. That way you could do 'move !x Pixie_inventory' and 'move !y Frank_inventory'. But you don't have to, you could just save the modifier of the first inventory to your backup register bak with 'set bak !x' after you received the first inventory listing. Then read the second inventory and work with both list expansion '!x' and backup expansion with '&'.

Another example would be to query the list of items first, then look up a single element with 'find !3' to populate the @ register with the name of the room that item is currently in, then use that @ register to slurp there using 'move room @'.

### Storing info on a floppy disk or the clipboard
If you hold a floppy disk in your hands while using the Hacker Board, the output of commands will be saved to that disk as comments by default. The comments will include a preceding empty line, some framing characters and possibly some header information, so they can be quite verbose. If you don't want those information to end up on your floppy, 'set std off' first. 'std' is short for 'save to disk' in case you wondered. But having this information stored on a disk might come in handy, so I leave it on usually, as it will only have an effect if you happen to have a floppy in your hands to start with. Just keeping a disk in your inventory while interacting with the board will not write anything to the disk in any case.

You can also enable an automated copy to clipboard functionality ('ctc' for short). To do so, 'set ctc on'. Changing this setting will put any result to the clipboard, overwriting the previous contents of the clipboard. This might not be the best in most cases, so I advise to 'set ctc off' again after you got what you were interested in, or just explicitly copy one specific item using the copy command like this: 'copy !0' to copy the first element of the current list to the clipboard. This will work regardless of the state of the global ctc register, so you can even use the copy command if ctc is turned off.

## Installation
1. Put the code from SummonHackerBoard.sprak into any item you want to use to toggle the board on or off with. I prefer to use the "CheckBalance()" function of Sebastian's CreditCard, but that requires an unrestricted modifier. You can use anything that is able to connect to something else, like a radio or a coke. CreditCards have the added benefit to not interact with trash cans accidently, I really like that.
2. Use the item you just hacked to bring up the Hacker Board, which at first will "just" be the Playstation from the mayor's Apartment. This machine was chosen because both of the nice big screen and the fact that it resembles a floating screen in the sky when summoned. It also comes with all the needed APIs enabled by default. If you want to use another computer, use a screwdriver to enable the APIs for internet, door, memory and floppy. And of course, change the connection in the summon script in step one.
3. Copy the code from EliteHackerBoard.sprak to the Playstation that just appeared out of nowhere, replacing all its original code. Don't worry, you will still be able to do Yulian's side quest to spy on Longson as the Hacker Board is able to run the original Playstation code. Just run the "playstation" command at the prompt.
4. You are basically done now, but you can of course change the "ext = Connect(..." line in the Setup() function to point to the extractor you are using. You should also check the registers by typing 'get all' and even set them to values you prefer.
