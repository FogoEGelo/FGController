# FGController
> A Luau library that can bring your Moon Animator plugin animations to life easily and conveniently, this includes:
> <img src="icon.png" align="right"/>

- **VFXs/Meshs** - And play the VFXs/MeshParts animations using the [VFX Forge](https://devforum.roblox.com/t/plugin-vfx-forge-an-advanced-custom-vfx-system/3867553).
- **BaseParts changes** - For example, CFrame, Color or transparency.
- **Screen changes** - For example, subtitles or vignette.
- **Camera changes** - For a cutscene for example.

## Installation

Just [download](#Download) the Library and place him in any service that the client can access, the default is the ReplicatedStorage.

## How to use

The library includes 2 Modules Sripts:

- **FGController** - The module that really makes the animations here.
- **AbilityCreator** - The other module that helps you to bring your ability life using the FGController.

You do not need to use FGController directly; instead, use AbilityCreator, as it will assist you in any situation.

### Adding your animation from Moon Animator
Create a folder in the ReplicatedStorage with the name "Animations".

In the Moon Animator you will save your animation, after that the save will appear in the ServerStorage -> MoonAnimator2Saves,
you will **copy and paste** the save to the folder who you created.

This save you pasted into the folder is the **AnimationFolder** remember this; and the path to him is:
```lua
RS.Animations:WaitForChild("AnimationFolderName") -- Change the AnimationFolderName to your save name.
```

### Taking the *Objs table*
The code which this returns is usefull to get the order of the objs to create the Objs Table if you want to play the animation
in another part but with the same animation, what else just copy and paste the code which this returns.

Just copy and paste this in the command bar or in a localscript and run.

```lua
local RS = game:GetService("ReplicatedStorage")

local ModelName = "ModelName" -- This is the name of the variable.
local AnimationsFolder = nil -- Change this to your AnimationFolder path.

local AbilityCreator = require(RS.Modules:WaitForChild("AbilityCreator"))
AbilityCreator.PrintSetupTemplate(AnimationsFolder, ModelName, true) -- This will print in your output a message with the code to copy and paste.
```

### Playing the animation
In a localscript you will copy and paste this:

```lua
local RS = game:GetService("ReplicatedStorage")

local AnimationsFolder = nil -- Change this to your AnimationFolder path.

local AbilityCreator = require(RS.Modules:WaitForChild("AbilityCreator"))
local Objs = {} -- Change this to the code who you copy from the last code.

AbilityCreator.Create(AnimationsFolder, Objs)
```

## API

