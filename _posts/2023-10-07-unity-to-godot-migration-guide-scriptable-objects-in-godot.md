---
layout: post
title:  "Unity To Godot Migration guide: ScriptableObjects in Godot (Resource!)"
date:   2023-10-07
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---

Scriptable Objects are a super useful feature in Unity.

Godot has an equivalent - the Resource object.  This guide gets into a bit: <https://docs.godotengine.org/en/stable/tutorials/scripting/resources.html#creating-your-own-resources>

Here's how you do it in a nutshell though - 

1. Create a new script that extends the Resource class.  For example "BaseTile"
2. In Godot, right click somewhere in your FileSystem window > Create New > Resource
3. in the popup window, select Resource and click Create
4. a Save dialog pops up, choose where and what you want your 'scriptable object' file to be called, and click save
5. in the FileSystem window, select the file that was created and go to the Inspector
6. you should see Script field that is `<empty>` - use the drop down, select 'Quick Load', choose your script.
7. that's it!

you can add `[Export]` properties to your script, drag stuff into your object, etc now.

```csharp
public partial class BaseTile : Resource
{
    [Export]
    public int Health { get; set; }

    [Export]
    public Resource SubResource { get; set; }

    [Export]
    public string[] Strings { get; set; }

    // Make sure you provide a parameterless constructor.
    // In C#, a parameterless constructor is different from a
    // constructor with all default values.
    // Without a parameterless constructor, Godot will have problems
    // creating and editing your resource via the inspector.
    public BaseTile() : this(0, null, null) {}

    public BaseTile(int health, Resource subResource, string[] strings)
    {
        Health = health;
        SubResource = subResource;
        Strings = strings ?? System.Array.Empty<string>();
    }
}    
```

![inspector for a custom resource](/docs/assets/images/godot-resource.png)