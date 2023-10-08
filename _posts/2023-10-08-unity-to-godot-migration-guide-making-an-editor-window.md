---
layout: post
title:  "Unity To Godot Migration guide: Making an Editor Window"
date:   2023-10-07
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
- GodotTools
---

In Unity, it's quite easy to make a custom EditorWindow - you pretty much just make a new class that extends EditorWindow, and create some way of opening the EditorWindow.  From there, you can draw whatever gui you need in the OnGUI method - very simple.

Godot has an equivalent, but the set up for this is more involved.  In Godot, they are called custom Docks.

The documentation provides an example, which I will be paraphrasing here as well: <https://docs.godotengine.org/en/stable/tutorials/plugins/editor/making_plugins.html#a-custom-dock>

In a nutshell, you will:
1. use Godot's plugin system to create a new Dock
2. create a new scene to build your Dock GUI
3. make your plugin load your scene into the Dock.
4. enable your plugin in Project Settings


## Use Godot's plugin system to create a new Dock
1. go to Project > Project Settings... > Plugins tab > Create New Plugin

![new plugin panel](/docs/assets/images/godot-plugin-create-new.png)

2. provide a name (e.g. "MyFirstDock")
3. provide a subfolder name (e.g. "MyFirstDock").  Note that it will be saved to `res://addons/MyFirstDock`
4. provide a script name (e.g. "MyFirstDock").  this will autogenerate the script you need at `res://addons/MyFirstDock/MyFirstDock.cs`
5. click `Create` - a script and a cfg file will be created in `res://addons/MyFirstDock`

![new plugin panel](/docs/assets/images/godot-plugin-new-settings.png)

### Warning after Creation
After clicking Create, you may be presented with a weird warning window, and some complaints in your log like 

`Another resource is loaded from path 'res://addons/MyFirstDock/MyFirstDock.cs' (possible cyclic resource inclusion).`

![new plugin panel](/docs/assets/images/godot-plugin-create-warning.png)

Just ignore this stuff for now.  Not the best advice, but it seems to be the editor just complaining once.


## Create a new scene to build your Dock GUI
1. create a new scene in godot
2. for your Root Node of the your scene, select `User Interface` - this will create a Control node at the root of your scene.
3. rename the root node to `MyFirstDock`
4. add a Button node as a child, give it a label like 'hello world' or whatever
5. save the scene here `res://addons/MyFirstDock/MyFirstDock.tscn`
6. let's attach a NEW script to the root node called `MyFirstDockGUI`

![MyFirstDockGUI](/docs/assets/images/godot-plugin-create-gui-script.png)

Your hierarchy should now look like this

![MyFirstDockGUI](/docs/assets/images/godot-plugin-scene-hierarchy.png)

7. stub out some test code for `MyFirstDockGUI` and link up the TestButton in the inspector

```csharp
#if TOOLS
using Godot;

[Tool]
public partial class MyFirstDockGUI : Control
{
    [Export] public Button TestButton;
    
    public override void _Ready()
    {
        base._Ready();
        TestButton.Pressed += OnTestButtonPressed;
    }

    public override void _ExitTree()
    {
        base._ExitTree();
        TestButton.Pressed -= OnTestButtonPressed;
    }
    
    private void OnTestButtonPressed()
    {
        GD.Print("OnTestButtonPressed!");
    }
}
#endif
```

this provides a TestButton export - remember to link this up in the Inspector GUI.

Save your scene, we're almost ready.

## make your plugin load your scene into the Dock.

Update your MyFirstDock script to load the MyFirstDock scene

```csharp
#if TOOLS
using Godot;

[Tool]
public partial class MyFirstDock : EditorPlugin
{
    private Control dock;
    
    public override void _EnterTree()
    {
        // Initialization of the plugin goes here.
        dock = (Control)GD.Load<PackedScene>("addons/MyFirstDock/MyFirstDock.tscn").Instantiate();
        AddControlToDock(DockSlot.LeftUl, dock);
    }

    public override void _ExitTree()
    {
        // Clean-up of the plugin goes here.
        // Remove the dock.
        RemoveControlFromDocks(dock);
        // Erase the control from the memory.
        dock.Free();
    }
}
#endif
```

## enable your plugin in Project Settings

This is easy - 

`Project > Project Settings... > Plugins tab > click enable on your plugin.`

you should now see your Dock, and clicking it should log your message!

![Success](/docs/assets/images/godot-plugin-scene-hierarchy.png)

NOTE: every time you change your scene or plugin code, you will need to toggle your plugin off, then on again.

