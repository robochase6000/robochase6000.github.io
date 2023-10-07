---
layout: post
title:  "Unity To Godot Migration guide: Fixing Blurry Pixel Art"
date:   2023-10-07
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---

In Unity, to ensure crisp pixel art, you need to start with some texture settings:

1. you need to make sure your image is being treated as a Sprite
2. your image needs to be set to 'Compression' 'None'
3. your texture filter needs to be set to 'Point'

This is a very similar process in Godot, however Godot provides a way to set the filter project wide, and per texture.

Just to illustrate the issue we're trying to solve.  we've made this pixel art texture atlas in photoshop -

![texture atlas in photoshop](/docs/assets/images/godot-blurry-tilemap-raw.png)

You bring it into Godot and it looks blurry - 

![texture atlas in photoshop](/docs/assets/images/godot-blurry-tilemap.png)

let's start by showing where to set this per texture
1. Select the node using the texture. In my case, this is a Tilemap node.
2. in the Inspector, scroll down to Canvas Item, and expand the Texture setion
3. the Filter option is probably set to 'Inherit', which means it's inheriting the setting from the project.
4. you can change the Filter option to 'Nearest' to make that texture crispy again!

![texture atlas in photoshop](/docs/assets/images/godot-blurry-canvas-item-fix.png)

We don't want to keep setting this for every texture if we don't have to.  Let's fix this in project settings
1. Project > Project Settings...
2. Rendering > Textures
3. change the 'Default Texture Filter' to 'Nearest'

![texture atlas in photoshop](/docs/assets/images/godot-blurry-project-settings.png)


now everythign should be nice and crispy again

![texture atlas in photoshop](/docs/assets/images/godot-blurry-tilemap-fixed.png)