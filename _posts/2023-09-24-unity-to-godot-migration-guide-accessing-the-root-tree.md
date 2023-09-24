---
layout: post
title:  "Unity To Godot Migration guide: Accessing the root of the tree"
date:   2023-09-24
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
This is a simple one actually. If you need to get at the root of the hierarchy, see `Node.GetTree().Root`

For example, here we can instantiate a Character to the root of the hiearchy - 

```csharp
public class CharacterSpawner : Node
{
    [Export] private PackedScene _characterPrefab;

    public override void _Input(InputEvent evt)
    {
        var justPressed = evt.IsPressed() && !evt.IsEcho();
        if (Input.IsKeyPressed(Key.Space) && justPressed)
        {
            Character instance = _characterPrefab.Instantiate<Character>();
            GetTree().Root.AddChild(instance);
            
            GD.Print("_Input() Spawned Character");
        }
    }
}
```

GetTree is a method of Node that returns the SceneTree.  You can read more about the SceneTree here - 
<https://docs.godotengine.org/en/stable/classes/class_scenetree.html>
