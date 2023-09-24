---
layout: post
title:  "Unity To Godot Migration guide: Browse runtime hierarchy"
date:   2023-09-23
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
![In an earlier post](http://robochase6000.github.io/2023/09/23/unity-to-godot-migration-guide-browse-runtime-hierarchy.html), I described how you can see browse the runtime hierarchy in Godot for inspection.

Wouldn't it be great if it entered this mode every time you ran the game, instead of needing to click back into Godot, and clicking "Remote" again?

There is actually an Editor setting for this - 

1. run the menu commande `Editor > Editor Settings...`
2. select the `Debugger` tab on the left
3. turn on `Auto Switch To Remote Scene Tree`

That's it.

![Auto Switch to Remote Scene Tree setting](/docs/assets/images/godot-auto-switch-to-runtime-scene-tree.png)
