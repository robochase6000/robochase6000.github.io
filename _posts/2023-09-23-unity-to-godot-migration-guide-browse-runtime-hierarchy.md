---
layout: post
title:  "Unity To Godot Migration guide: Browse runtime hierarchy"
date:   2023-09-23
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
In Unity, you get both a separate scene and game window in the editor.  While the game is running, you can examine the hiearchy, select game objects, and view selected game objects in the inspector.

You can do this in godot too, but it's not immediately obvious how.

The trick is to run the game from the godot editor, then go back to the editor and look at the Scene window - instead of viewing Local, click Remote, you should now be able to inspect the runtime hierarhcy of your game.

![Our new spawn timer fields in the inspector](/docs/assets/images/godot-runtime-inspector.png)


As of Godot 4.1, this seems to be as good as it gets.  You can't currently click around the scene like you can in unity and pan the scene camera around.

Note that I disovered a setting that lets the editor enter Remote' mode automatically when you start playing the game - <http://robochase6000.github.io/2023/09/24/unity-to-godot-migration-guide-auto-switch-to-runtime-hierarchy.html>

