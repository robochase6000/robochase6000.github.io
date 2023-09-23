---
layout: post
title:  "Unity To Godot Migration guide: button click callbacks in code"
date:   2023-09-23
tags:
- Unity
- Godot
- UnityToGodotMigrationGuide
---
Often we need to know when a button is clicked. The tutorials in Godot guide you toward setting up a signal that gets sent to your script.  This is annoying to wireup.  This is actually quite simple to do in code - 

```
public partial class MyButton : Button
{
    public Action<MyButton> OnPressedCallback;
    
    public override void _Ready()
    {
        this.Pressed += OnPressed;
    }

    private void OnPressed()
    { 
        GD.Print("MyButton.OnPressed!");
        OnPressedCallback.Invoke(this);
    }
}
```

