This documentation assumes you already have the basics of making OpenRCT2 json objects down.
When this guide refers to "peep", it is referring to both guests and staff as they are both considered peeps internally in OpenRCT2.
Presently this documentation only covers making custom entertainers, some things in this guide may or may not apply to custom guests, handymen, mechanics, or security guards.

For peep animation properties, there are two options.

"peepType" determines the type of peep this object is for. Since this is documentation for entertainers the template object is set to "entertainer"
"noRandomPlacement" is a bool that determines whether this peep animation object is available for random selection when spawning entertainers. The template object defaults to true so random placements are disabled, change to false if you want random selection for your object.

Peep animation images are structured in a simple way, do note that OpenRCT2 counts from 0 for these things, so the first image will be image 0.

The very first image (image 0) is the icon that shows up in the object selection window & costume selection drop downs. The template object includes a 16x10 placeholder icon for convenience.
For making a custom icon, the size most entertainers use is 16 pixels wide but you can make it wider or thinner if need be. Do not make it taller as having it taller than 10 pixels makes it taller than the costumes drop down menu.

After this is every sprite the actual peep entity has, for most animation sequences this will be in sets of four sprites with NE, SE, SW, and NW facing angles in that order, then following that is the next frame of the animation. These are similar to animated small scenery objects.
The only sprites that do not have directions are the "hanging" sprites, although in most cases you can just ignore this as it's not a required set of sprites as the template object is set up to use the first SW facing walking frame by default.

For convenience, an images table with walking, wave, and joy animations with support for up to 200 sprites in each of them as been provided in the template object. Simply trim it down to suit the needs of your object.

Lastly are the "animationGroups" which define what and how many sprites are used for playing the set animations.

The "isSlowWalking" bool is defined by "animationGroups" firstly, this determines whether the peep will move and animate slowly. Defaults to false, set to true if you want slower peeps. This applies to all animations, not just walking.

The two most important parameters for "animations" entries are the following:
"offset" determines where in the images table index the current animation will get it's sprites from. This effectively counts from 1 as image 0 always has to be the icon.
To get the proper "offset" value, you can just add the number of previous all image groups together (eg: 1 icon + 24 walking images + 12 wave images will have an offset of 37 for joy images)

"sequence" is a table that defines the exact sequence of the current animation. Each number counting from 0 will display a different frame from the current animation index.
This does not count rotational frames for every group except for hanging, so simply divide the number of frames your animation has by 4 in most cases to get the numbers you should plug into this table.
Keep in mind that you can put these in any order, and even duplicate them. An order of "0, 1, 2, 3, 2, 1, 0" is valid for example and will result in an animation that goes forwards, then backwards.

The following animations are the ones you will usually be modifying for custom entertainers:
"walking" is the animation used for walking, you'll obviously want to keep this as a walking animation but you should generally try to keep the number of frames for it reasonable. As the game will only animate this and other animations at a fixed "normal" or "slow" speed depending on your slow walking bool.
"wave2" & "joy" can both be effectively treated as a random animation that plays every few seconds with a roughly 50/50 chance for either one to occur. This can be used for pretty much anything so have fun with it!
If you don't want to have a "joy" animation just copy all of your parameters from "wave2" and it will work fine.
If you don't want to have either random animation just leave them unchanged, they default to one tick of the first walking animation so this will effectively disable them. This will cause a very short one tick long pause/stutter but there's no other way to do this presently.

For the most part you can ignore the following animations for custom entertainers as they are not really applicable in most cases and default to a sensible alternative in the template object:
"watchRide" is an unused standing animation. The template object defaults to the first walking animation frames.
"hanging" is the animation that plays when you pick up a peep and they hang from the tweezers. This is the only animation that has no rotations. The template object defaults to the first SW frame of the walking animation.
"drowning" is the animation that plays when a peep is drowning. The template object defaults to the first walking animation frames, this will only last for a single tick as the peep will drown nearly instantly.