---
layout: post
title:  "Unity To Godot Migration guide: Node Lifecycle"
date:   2023-09-23
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
In Godot, a Node has a bunch of life cycle events, similar to Unity how a MonoBehaviour has Awake, Start, Update, FixedUpdate, OnDestroy.

This isn't exhaustive, as I'm still pretty new to Godot, but here's my findings so far.

## Initialization
1. Node Constructor is called
2. `_EnterTree` is called
3. `_Ready` is called

## Updates
1. `_PhysicsProcess(double delta)` is basically FixedUpdate.  it seems to get the same delta passed in every frame.
2. `_Process(double delta)` is basically Update.  it should get different deltas passed in every frame

# Destruction
1. `QueueFree();` is basically the same as calling `Destroy(gameObject);`
2. `_ExitTree` is all could i find as an equivalent to `_OnDestroy`


You can try this out yourself.  this node will execute for a brief period of time and then remove itself.

```
public partial class TestNode : Node
{
    private double _timeAlive = 0f;
    [Export] public double Lifetime = 0.2f;
    
    public TestNode() : base()
    {
        GD.Print("TestNode()");
    }

    public override void _EnterTree()
    {
        GD.Print("TestNode._EnterTree()");
    }
        
    public override void _Ready()
    {
        GD.Print("TestNode._Ready()");
    }
        
    public override void _PhysicsProcess(double delta)
    {
        GD.Print($"TestNode._PhysicsProcess(delta:{delta})");
    }
        
    public override void _Process(double delta)
    {
        GD.Print($"TestNode._Process({delta}) -> timeAlive:{_timeAlive}");
        _timeAlive += delta;
        if (_timeAlive >= Lifetime)
        {
            GD.Print($"TestNode._Process({delta}) -> Deleting self!");
            QueueFree();
        }
    }
        
    public override void _ExitTree()
    {
        GD.Print("TestNode._ExitTree()");
    }
}
```

My debug output:

```
TestNode()
TestNode._EnterTree()
TestNode._Ready()
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._Process(0.11666666666666667) -> timeAlive:0
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._PhysicsProcess(delta:0.016666666666666666)
TestNode._Process(0.01666666666666667) -> timeAlive:0.11666666666666667
TestNode._Process(0.01666666666666667) -> Deleting self!
TestNode._ExitTree()
```
