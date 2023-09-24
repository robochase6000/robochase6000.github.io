---
layout: post
title:  "Unity To Godot Migration guide: Object Instantiation"
date:   2023-09-24
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
In Unity we might store a reference to a prefab, and instantiate an instance like this - 

```csharp
public class CharacterSpawner : MonoBehaviour
{
    [SerializeField] private Character _characterPrefab;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space)) 
        {
            Character instance = Instatiate(_characterPrefab);

            Debug.Log("Update() Spawned Character")
        }
    }
}
```

In Godot, it's fairly similar.

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
            AddChild(instance);
            
            GD.Print("_Input() Spawned Character");
        }
    }
}
```

Note that we're using Godot's `Node._Input` method instead of `_Process` because Godot doesn't have such a thing as `Input.IsKeyJustPressed`, so here we're checking ourselves. This limits it to spawning one Character per key press, instead of one Character every frame the key is held down.

Also note that the above code will make the character a child of the node spawning it.  `AddChild` is a method of `Node`, so you could easily add it to a different node.  Or if you wanted to add it to the root of the hierachy, see `Node.GetTree().Root` - GetTree is also a method of Node, and it returns the SceneTree.