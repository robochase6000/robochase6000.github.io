---
layout: post
title:  "Use Jetbrains Rider as default text editor for Godot on Mac"
date:   2023-09-23
tags:
- Godot
- Rider
---
the documentation from Godot 4.1 isn't very clear - https://docs.godotengine.org/en/stable/tutorials/editor/external_editor.html

Here's how to do it on the mac -
Editor > Editor Settings > Text Editor > External

See the Exec Path field - 
![Setting up Rider in Godot](/assets/images/godot-rider-config.png)

you need to navigate to inside the Rider.app -> select Rider.app/Contents/MacOS/rider

this only gets so far.  with this setting changed, double clicking a file in Godot will open the file in Rider, but it won't open the sln in rider.

To have it auto-open the sln in rider, enter this in the Exec Flags field - 
`{project} --line {line} {file}`