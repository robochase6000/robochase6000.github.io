---
layout: post
title:  "Unity To Godot Migration guide: use Export instead of SerializeField"
date:   2023-09-23
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
A MonoBehaviour in Unity is basically a Node in Godot.

When writing a script that Node, you can use the [Export] attribute in Godot the way you would use [SerializeField] in Unity.

For example in Unity you might write stuff like this in your MonoBehaviour to expose fields in the unity inspector - 

```
[SerializeField] private Timer _spawnTimer1;
private Timer SpawnTimer2;
```

In your godot script, you can do this - 


```
[Export] public Timer SpawnTimer1_PublicProperty { get; set; }
[Export] public Timer SpawnTimer2_PublicField;
[Export] private Timer _spawnTimer3_PrivateProperty { get; set; }
[Export] public Timer _spawnTimer4_PrivateField;
```

this creates new fields in the inspector you can drop other nodes into, just like unity - 

![Our new spawn timer fields in the inspector](/docs/assets/images/godot-export-inspector.png)

