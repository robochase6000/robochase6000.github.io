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

You need to hit all 3 settings here - 

1. Use External Editor: On
2. Exec Path - you need to navigate to the executable _inside_ the Rider.app -> select Rider.app/Contents/MacOS/rider
3. Exec Flags - change to 
{% highlight %}
{project} --line {line} {file}
{% endhighlight %}

In the end you should look kinda like this - 
![Setting up Rider in Godot](/docs/assets/images/godot-rider-config.png)

Now double clicking scripts in Godot should open them in Rider correctly.