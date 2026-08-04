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

### Taking the Objs table
Just copy and paste this in the command bar or in a localscript and run.

```lua
local RS = game:GetService("ReplicatedStorage")

local ModelName = "ModelName" -- This is the name of the variable that's you defined.
local AnimationsFolder = RS.Animations:WaitForChild("AnimationName") -- Rename the AnimationName to your animation name on the Animations folder.

local AbilityCreator = require(RS.Modules:WaitForChild("AbilityCreator"))
AbilityCreator.PrintSetupTemplate(AnimationsFolder, ModelName, true) -- This will print in your output a message with the code to copy and paste.
```

## API

