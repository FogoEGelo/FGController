<div align="center">
  <img alt="FGController" title="FGController" src="icon.png" width="95%" />
  <br />
  <br />
  <a href="[https://github.com/FogoEGelo/FGController-System/tree/main](https://github.com/FogoEGelo/FGController-System/tree/main)">
    <img alt="Download" title="Download" src="Button.png"/>
  </a>
  <a href="[https://discord.gg/8bqztkDA2U](https://discord.gg/8bqztkDA2U)">
    <img alt="Discord" title="Discord" src="Button2.png"/>
  </a>
</div>

<br />

# FGController
> A powerful, strictly-typed Luau library that easily and conveniently brings your Moon Animator plugin animations to life in Roblox.

<div align="center">
  <img width="400" height="225" alt="Showcase 1" src="[https://github.com/user-attachments/assets/ade769e7-ec7e-40d1-ba9c-65f3785e7c7d](https://github.com/user-attachments/assets/ade769e7-ec7e-40d1-ba9c-65f3785e7c7d)" />
  <img width="400" height="225" alt="Showcase 2" src="[https://github.com/user-attachments/assets/05041d8d-d28f-4f3a-acda-9c2ff08f8efc](https://github.com/user-attachments/assets/05041d8d-d28f-4f3a-acda-9c2ff08f8efc)" />
  <br /><br />
  <strong>💎 Premium Module</strong><br />
  This library is a paid asset and costs <strong>$15 USD / R$ 75</strong>.<br />
  To purchase and get repository access, please send a DM or open a ticket on our <a href="[https://discord.gg/8bqztkDA2U](https://discord.gg/8bqztkDA2U)">Discord</a>.<br />
  🎥 <a href="[https://www.youtube.com/watch?v=AGdijNJ8wA0](https://www.youtube.com/watch?v=AGdijNJ8wA0)">Watch the Tutorial/Example</a>
</div>

---

## 🌟 Features

This library allows you to seamlessly synchronize and play back:
* **VFX & Meshes** - Play VFX/MeshPart animations perfectly timed using [VFX Forge](https://devforum.roblox.com/t/plugin-vfx-forge-an-advanced-custom-vfx-system/3867553).
* **BasePart Properties** - Animate `CFrame`, `Color`, `Size`, and `Transparency` dynamically.
* **Screen UI Elements** - Automatically manages and animates `ScreenGui` elements like Subtitles (`TextLabel`), Letterboxes (`Frame`), or Vignettes (`ImageLabel`).
* **Camera Movement** - Create synchronized cutscenes effortlessly with advanced `RelativeA` and `CameraTarget` tracking.
* **✨ Custom Commands System** - The engine features a smart Regex reader. You are no longer limited to `shared.vfx.emit()`. You can now trigger custom functions like `playSound()` or `cameraShake()` directly from Moon Animator's event tracks.
* **🛡️ Bulletproof Engine** - Written in Luau `--!strict` mode with anti-memory leak protections. It safely resets cameras and UI elements *only* if the specific ability used them.

---

## 📥 Installation

[Download](https://github.com/FogoEGelo/FGController-System/tree/main) the library and place it in any service accessible by the client. The recommended location is `ReplicatedStorage.Modules`.

---

## ⚠️ Important Observations

* **Anchor your parts:** All Meshes/Parts have to be anchored to play correctly. It is highly recommended to disable `CanCollide` as well.
* **Naming Convention:** Do not name any instance with just numbers (e.g., a MeshPart named "1"). Moon Animator fails to save these correctly.
* **Client-Side Only:** This module must be required and executed on the **Client** (LocalScript) to handle the Workspace Camera and PlayerGui properly.
* **Event Codes:** Custom codes in Moon Animator events **must be placed in "Code Begin"**, not "Code End".
* **🎥 Local Camera Setup:** For the camera to animate correctly relative to a player/part:
  1. In Moon Animator, place the main character/part exactly at position and orientation `(0, 0, 0)`.
  2. Animate the camera around this `(0, 0, 0)` character.
  3. In your script, pass the real in-game character's `CFrame` into the `RelativeA` parameter. The module will automatically translate the animation to your character's current world position and facing direction.

---

## 🏗️ Core Structure

The library includes two main ModuleScripts:

1. **`FGController`** - The core engine that calculates absolute time (`os.clock()`), syncs Tweens, parses Custom Commands, and runs the animations.
2. **`AbilityCreate`** - A user-friendly wrapper that parses your Moon Animator JSON data, sets up the UIs/Cameras automatically, and plays your abilities using the controller.

> **Note:** You usually do not need to interact with `FGController` directly. `AbilityCreate` handles all the heavy lifting and is the primary module you should use in your scripts.

---

## 🚀 Quick Start Guide

### 1. Adding your animation from Moon Animator
Create a folder inside `ReplicatedStorage` named `Animations`.

When you save your animation in Moon Animator, the save file will appear in `ServerStorage -> MoonAnimator2Saves`. **Copy and paste** this save file into the `Animations` folder you just created.

### 2. Generating the Setup Template
Because your animation relies on specific objects (VFX, Parts, UIs), `AbilityCreate` needs a table linking the Moon Animator tracks to the real instances in your game. We built an automated tool to generate this table for you!

Run this code in the **Command Bar** (Bottom of Roblox Studio):

```luau
local RS = game:GetService("ReplicatedStorage")
local AbilityCreate = require(RS.Modules:WaitForChild("AbilityCreate"))

local AnimationsFolder = RS.Animations:WaitForChild("YourAnimationSaveName") 
local ModelName = "Character" -- The variable name of the character/model in your script

-- This will print a ready-to-use template in your Output (F9)
AbilityCreate.PrintSetupTemplate(AnimationsFolder, ModelName, true) 
```

Check your Output window, copy the generated code, and paste it into your actual LocalScript.

### 3. Playing the Animation
Once you have your `Objs` table set up, playing the animation requires just one line of code:

```luau
local RS = game:GetService("ReplicatedStorage")
local AbilityCreate = require(RS.Modules:WaitForChild("AbilityCreate"))
local AnimationsFolder = RS.Animations:WaitForChild("YourAnimationSaveName")

-- Paste the generated table here:
local Objs = {
    -- ... your generated objects ...
}

-- Play the ability!
local myAbility = AbilityCreate.Create(AnimationsFolder, Objs, 60, Character.PrimaryPart.CFrame)

-- If you ever need to stop it manually mid-cast:
-- myAbility:Stop()
```

---

## 🛠️ Custom Commands System (New!)

The V1.8+ Engine introduces a dynamic command registry. By default, the engine already recognizes commands like `shared.vfx.emit(Object)` and `print("text")` placed in Moon Animator's Event tracks.

To add your own custom logic (like playing sounds or shaking the screen):
1. Open the `FGController` module.
2. Locate the `Commands` table.
3. Add your custom function. The engine's Regex will automatically detect it and parse the arguments for you!

---

## 📚 API Reference

### `AbilityCreate` (Main Wrapper)

| Method | Parameters | Returns | Description |
| :--- | :--- | :--- | :--- |
| **`.Create()`** | `Folder: StringValue`<br>`Objs: {Instance}`<br>`FPS: number?`<br>`RelativeA: CFrame?`<br>`CameraTarget: BasePart?`<br>`CameraOrigin: CFrame/Vector3?` | `self` | Initializes and immediately plays the sequence. Binds UIs, VFXs, and Camera safely. Default FPS is `60`. |
| **`:Stop()`** | None | `self` | Instantly cancels all running tweens, stops loops, clears memory, and selectively hides UI/Camera *only* if they were used by this ability. |
| **`.PrintSetupTemplate()`**| `Folder: StringValue`<br>`ModelName: string`<br>`wait: boolean?` | `nil` | Prints a formatted Lua table to the Output window. Set `wait` to `true` to use `:WaitForChild()`. |
| **`.getObjsOrder()`** | `Folder: StringValue` | `{string}` | Returns a sequentially ordered table of string names representing the exact object order required. |
| **`.PlayCharAnimation()`** | `Anim: Animation`<br>`Char: Model` | `AnimationTrack` | Helper function to load and return a player animation safely. Call `:Play()` on the returned track. |

<br />

### `FGController` (Internal Engine)

| Method | Parameters | Returns | Description |
| :--- | :--- | :--- | :--- |
| **`.new()`** | `object: Instance`<br>`fps: number?`<br>`cameraOrigin: CFrame?`<br>`cameraTarget: BasePart?`<br>`relativeA: CFrame?` | `self` | Creates a new controller instance tied to a specific object. |
| **`:SetAnimation()`** | `Folder: StringValue`<br>`Config: Config?` | `self` | Binds the animation folder to the object. Accepts an optional config table to toggle specific properties (`LocalPos`, `LocalCamera`, etc). |
| **`:Play()`** | `Loop: boolean`<br>`Objs: {Instance}?` | `self` | Starts the animation in a parallel thread (`task.spawn`), utilizing strict `os.clock()` calculations to prevent frame drift. |
| **`:CallBack()`** | `CallBack: () -> ()` | `self` | Connects a function to fire exactly when the animation finishes naturally or is manually stopped. |
| **`:Stop()`** | None | `self` | Cancels all active `TweenService` tracks for this specific object and halts the time loop. |

---

## 📄 License & Terms

**VFX Forge Developer License (VFX-DL) Version 1.0**

To buy the license to use this module, please send a message on Discord to purchase it.
*You must have a GitHub account to get permission to access and download the repository.*

**Updates:** This module is updated frequently. If you notice an error, feel free to send a message or create an issue/comment on the repository. The codebase is actively maintained and bugs are fixed promptly.
