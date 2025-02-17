## Changes/Additions:
* More accurate and good looking lighting/shading.
* `Viewport Config` now renamed to `Toolbox`. It is referred to as such within this changelog as well.
* DummyPoly ID text is now closer to the actual location 
* DummyPoly 200 shows regular ID text on all points instead of just one that says "(all over body)"
* New DummyPoly menu in `Toolbox`. Has a list of DummyPoly for each model. Hovering over them shows just that DummyPoly in the viewport allowing you to tell where it is.
  * Click a DummyPoly to toggle it on/off. If off nothing related to it will show. Even SFX spawns etc will not show from that DummyPoly. The only exception is hitbox shapes which will always show entirely with hitbox simulation enabled.
* Some helper colors changed around.
* New option `Scene -> Show c0000 Weapon Global DummyPoly ID Values (10000+)`, which displays the globally-offsetted values you would use for paired weapon / trick weapon AtkParams etc.
* Changed `Animation -> Lock to Frame Rate Defined in HKX` to use Math.Floor instead of Math.Round, which works more accurately with events during playback.
* `Scene -> Helper: Flver Skeleton` is now yellow and renders all bones with the appropriate orientation/length. Due to technical limitations it now displays as just a single line per bone like the debug menu option in the actual game engine.
* Animation blending now obeys the "lock to framerate defined in HKX" option.
* Manually selecting an animation either by clicking or my pressing Up or Down now cancels the currently playing combo if applicable.
* `PlaySound_ByStateInfo` event will now stop the sound when the event is no longer active. This allows it to fade out some looping sounds etc and works like it does ingame.
  * Many sounds cutoff harshly in DS1 because they seem to use some kind of weird hacky solution, but it's fine I guess.
* Grid and hitbox color are now the originally intended duller colors that match the ingame debug colors.
* New section in viewport config window named COLORS. Allows you to adjust grid color, hitbox colors, various helper colors, etc.
* New render modes to debug mesh UV coordinates, with an option for TEXCOORD0 and another for TEXCOORD1.
* Now loads human body and face for the player kinda. Does not look very good.
* Missing diffuse and specular textures are now gray instead of white, making things easier to see.
* New option `Simulation -> Simulate Character Tracking (A/D Keys)`
  * Preview character rotation speed.
  * Takes JumpTable 7 (DisableTurning) into account.
  * Takes Event 224 (SetTurnSpeed) into account.
  * Apply an automatic constant turn speed right at the top of `Toolbox`.
* Volume bar now displays in percent instead of multiplier.
* Volume bar maximum increased to 200%
  * New "Reset to 100%" button under volume.
* Some new separators added to Toolbox imgui menu for increased readability.
* Made memory usage text on bottom right smaller.
* You now have to zoom in less to see individual frame numbers.
* Camera default orbit point makes more sense on more models
* Lighting direction can now be set while lighting is set to follow camera, giving a sort of backlight effect etc.
  * New default has model lit from the side, giving a more dramatic look.
* Loading a new character now force stops all sound output (you can't manually end sound output until after it finishes loading the new character, so that was really annoying)
* Middle clicking to recenter the camera no longer resets the zoom.
* Entire camera system rewritten from scratch to use quaternions and also lots of spaghetti code removed. Along with that, are a few changes:
  * Camera can now be rotated freely vertically even past the top.
  * Skybox no longer rotates with the player model during rotational root motion.
  * Moving the camera now shows the pivot point in 3D space for a brief moment to let you better know what is happening.
  * The Ctrl+(+)/(-) binding for zooming the timeline in/out now works for the viewport. Might as well have the option; someone on the Discord server had a broken mouse wheel andd I felt bad lol.
  * Zooming now uses different math that makes it finer/slower near the pivot point and faster far away from it.
  * Zooming in too close to the pivot point now pushes the pivot point forward.
  * Clicking and dragging to rotate the camera / pan the camera has been rewritten: 
    * You can hold shift to go faster.
    * You can hold ctrl to make right click drag the camera orbit origin forward/backward instead of up/down.
    * Mouse cursor gets hidden and locked in place.
	  * This allows free movement in all directions without ever hitting the edge of your monitor.
    * Uses raw mouse input
	  * No mouse acceleration.
	  * The DPI of your mouse directly maps to the precision of movement in DS Anim Studio.
	  * Factors in your Windows mouse pointer speed as a base so users of high DPI mouse with low speed shouldn't have issues with it being way too fast by default.
	  * New overall mouse input speed multiplier in the Toolbox near the camera rotate/move speed values that were already there.
    * Middle clicking to reset the camera will now make it follow the character's lockon dummypoly automatically from that point onward, unless you right click and drag to move the pivot point.
    * New option to snap to 45 degree angles, located in the Toolbox.
* Resizing the application window maintains the same model viewer and inspector pane size ratios relative to the total size of the screen.
* New shader approximates PTDE shading in DS3's material setup, giving fairly decent (though not correct) looking lighting for PTDE models.
* You can make the viewport bigger than before.
* Animation list a bit more compact, can also be made smaller than before, giving more room for graph and viewport.
* Toolbox can now be dragged around.
* Toolbox contents can be scaled with 2 new options at the top.
* Viewport text (like weapon animations etc) can now be rescaled with a new bar near the top of Toolbox.

## Fixes:
* Fixed longstanding critical oversight where the individual normals from normal maps were not oriented correctly.
* Spawn events that use the global weapon model offsets (10000+) now display correctly.
* Fixed a bug where weapon locations displayed in the location of the previous animation frame for 1 render frame after changing animation frames before suddenly snapping to the right location. This was extremely noticable for very drastic location changes such as Bloodborne trick weapon transformation animations, where you would see the weapon in an extremely incorrect location for 1 frame and it appeared very jarring.
* Fixed bug where hitting Insert key to insert a new animation would scroll to a weird place in the animation list.
* Weapon animations now reset to frame 0 when the player's animation loops (useful for mods that are using a weapon animation that's longer than the player's).
* Fixed a bug from I dunno how long ago where model masks on player equipment were not ever applied.
* Zooming the graph way out no longer makes the timeline blank on top.
* Middle clicking to recenter camera now goes to the proper rotation if the model is root motion rotated and camera follows root motion rotation is on
* Fixed longstanding bug where playing an animation with rotational root motion then blending into an animation with lateral root motion would result in the model moving in a different direction than it is facing (sliding sideways on dashes etc)
* Fixed Edit Animation Properties being cutoff if you are using an abnormal windows DPI scale factor. Note that you must still do this if you use DPI scaling:
  * Right click `DS Anim Studio.exe`
  * Click `Properties`
  * Go to `Compatibility` tab
  * Go to `Change high DPI settings`
  * Check both checkboxes
  * Set lower scaling override dropdown to `Application`
  * Hit OK
  * Hit Apply 
  * Hit OK 
  * Close any running instance of DS Anim Studio and launch it again
* Sekiro Edit Animation Properties window no longer shows short animation ID format (aXX_YYYY) like DS1.
* Fixed bug where "Goto Event Source" button would internally set a value wrong causing it to fail to copy events to clipboard until you switched animations.
* Some instances of modifying files but not having an unsaved changes indicator have been fixed.
* Copy/Paste hotkeys in the inspector pane will no longer make the event graph freak out as if you're trying to copy/paste event data.
* Keyboard input in Toolbox no longer permanently stops working after loading a character, alt-tabbing, and various other actions.
  * You likely didn't realize it had keyboard input since it didn't work. Ctrl + Left Click on a slider value to do keyboard input for that value.