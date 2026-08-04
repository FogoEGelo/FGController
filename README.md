<div align="left">
<picture>
<img alt="" title="FGController" src="icon.png" width="95%"/>
</picture>

<div align="center">
<a href="https://create.roblox.com/store/asset/72718229058068/FGController"><img alt="" title="Download" src="Button.png"/></a>
<div align="left">

# FGController
> A powerful Luau library that easily and conveniently brings your Moon Animator plugin animations to life in Roblox.

This library allows you to seamlessly synchronize and play back:
- **VFX & Meshes** - Play VFX/MeshPart animations perfectly timed using [VFX Forge](https://devforum.roblox.com/t/plugin-vfx-forge-an-advanced-custom-vfx-system/3867553).
- **BasePart Properties** - Animate `CFrame`, `Color`, `Size`, and `Transparency` dynamically.
- **Screen UI Elements** - Control `ScreenGui` elements like subtitles, letterboxes, or vignettes.
- **Camera Movement** - Create synchronized cutscenes effortlessly.

---

## Installation

[Download](#Download) the library and place it in any service accessible by the client. The recommended location is `ReplicatedStorage`.

---

## Some observations
- All the Meshes/Parts have to be **anchored to play correctly** and it's recomended remove the CanCollide too.
- **Don't name any thing with numbers**, for example a MeshPart named with "1", because the Moon Animator don't save this correctly.
- **This module had to be used in client** to use the camera/UI correctly.
- **For now** the module just support the `shared.vfx.emit` in events with code.
- The codes in events **have to be in Code Begin**, not Code End, **for now.**

---

## Core Structure

The library includes two main ModuleScripts:

- **`FGController`** - The core engine that calculates time, syncs tweens, and runs the animations.
- **`AbilityCreate`** - A user-friendly wrapper that sets up and plays your abilities automatically using `FGController`.

> **Note:** You do not need to interact with `FGController` directly. `AbilityCreate` handles all the heavy lifting and is the primary module you should use.

---

## Quick Start Guide

### 1. Adding your animation from Moon Animator
Create a folder inside `ReplicatedStorage` named `Animations`.

When you save your animation in Moon Animator, the save file will appear in `ServerStorage -> MoonAnimator2Saves`. **Copy and paste** this save file into the `Animations` folder you just created.

This save file is your **AnimationFolder**. You can reference it in your scripts like this:
```lua
local RS = game:GetService("ReplicatedStorage")
local animationFolder = RS.Animations:WaitForChild("YourAnimationSaveName")
```

### 2. Generating the Setup Template (The `Objs` Table)
Because your animation relies on specific objects (VFX, Parts, UIs), `AbilityCreate` needs a table linking the Moon Animator tracks to the real instances in your game. We built an automated tool to generate this table for you.

Run this code in the **Command Bar** or a temporary test script:

```lua
local RS = game:GetService("ReplicatedStorage")
local AbilityCreate = require(RS.Modules:WaitForChild("AbilityCreate"))

local AnimationsFolder = RS.Animations:WaitForChild("YourAnimationSaveName") 
local ModelName = "Character" -- The variable name of the character/model in your script

-- This will print a ready-to-use template in your Output (F9)
AbilityCreate.PrintSetupTemplate(AnimationsFolder, ModelName, true) 
```
Check your Output window, copy the generated code, and paste it into your actual LocalScript.

### 3. Playing the Animation
Once you have your `Objs` table set up, playing the animation requires just one line of code in your LocalScript:

```lua
local RS = game:GetService("ReplicatedStorage")
local AbilityCreate = require(RS.Modules:WaitForChild("AbilityCreate"))

local AnimationsFolder = RS.Animations:WaitForChild("YourAnimationSaveName")

-- Paste the generated table here:
local Objs = {
    -- ... your generated objects ...
}

-- Play the ability!
local myAbility = AbilityCreate.Create(AnimationsFolder, Objs)

-- If you ever need to stop it manually:
-- myAbility:Stop()
```

---

## API Reference

### `AbilityCreate` (Main Module)
This is the primary module you will use to trigger animations and handle VFX logic.

| Method | Parameters | Returns | Description |
| :--- | :--- | :--- | :--- |
| **`.Create()`** | `AnimationsFolder: StringValue`<br>`Objs: {Instance}`<br>`FPS: number?` | `self` | Initializes and immediately plays the animation sequence. Binds UIs and VFXs automatically. Default FPS is `60`. |
| **`:Stop()`** | None | `self` | Instantly cancels all running tweens, stops the time loops, clears memory, and hides the UI elements. |
| **`.PrintSetupTemplate()`** | `AnimationsFolder: StringValue`<br>`ModelName: string`<br>`wait: boolean?` | `nil` | Prints a formatted Lua table to the Output window. Set `wait` to `true` to use `:WaitForChild()`, or `false` for `:FindFirstChild()`. |
| **`.getObjsOrder()`** | `AnimationsFolder: StringValue` | `{string}` | Returns a sequentially ordered table of string names representing the exact object order required by the animation file. |

### `FGController` (Internal Engine)
While `AbilityCreate` automates this, here is the API for the underlying engine in case you need to build custom, low-level implementations.

| Method | Parameters | Returns | Description |
| :--- | :--- | :--- | :--- |
| **`.new()`** | `Object: BasePart/GuiObject`<br>`FPS: number?` | `self` | Creates a new controller instance tied to a specific object. |
| **`:SetAnimation()`** | `AnimationFolder: StringValue`<br>`Config: Config?` | `self` | Binds the animation folder to the object. Accepts an optional configuration table to enable/disable specific properties, one of this properties is the `Localpos`, the default is true for this, if true this will change the world position from the Moon Animator to a local position designed by the actual CFrame of the part, if false this will uses the world CFrame from the Moon Animator. |
| **`:Play()`** | `Loop: boolean`<br>`Objs: {Instance}?` | `self` | Starts the animation in a parallel thread (`task.spawn`), utilizing strict `os.clock()` calculations for perfect frame syncing. |
| **`:CallBack()`** | `CallBack: () -> ()` | `self` | Connects a function to fire exactly when the animation finishes naturally or is stopped. |
| **`:Stop()`** | None | `self` | Cancels all active `TweenService` tracks for this specific object and halts the loop. |

## Limitations


## License
**License (VFX-DL) Version 1.0**

Read the start of the `AbilityCreate` or the start of the `FGController` to check the terms of uses.

## Updates
This module is updated frequently, if you notice an error send me a message, or `create a comment here`

Theses modules are recents, then probably have errors even i had fixed a lot
