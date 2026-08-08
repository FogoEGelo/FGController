<div align="center">
  <img alt="FGController" title="FGController" src="icon.png" width="95%" />
  <br />
  <br />
  <a href="https://github.com/FogoEGelo/FGController-System/tree/main">
    <img alt="Download" title="Download" src="Button.png"/>
  </a>
  <a href="https://discordapp.com/users/714085730430877716">
    <img alt="Discord" title="Discord" src="Button2.png"/>
  </a>
</div>

<br />

<h1>FGController</h1>
<blockquote>
  <p>A powerful Luau library that easily and conveniently brings your Moon Animator plugin animations to life in Roblox.</p>
</blockquote>

<div align="center">

  <img width="400" height="225" alt="Image" src="https://github.com/user-attachments/assets/ade769e7-ec7e-40d1-ba9c-65f3785e7c7d" />
  <img width="400" height="225" alt="Image" src="https://github.com/user-attachments/assets/05041d8d-d28f-4f3a-acda-9c2ff08f8efc" />
  
  ---

</div>

<p>This library allows you to seamlessly synchronize and play back:</p>
<ul>
  <li><strong>VFX & Meshes</strong> - Play VFX/MeshPart animations perfectly timed using <a href="https://devforum.roblox.com/t/plugin-vfx-forge-an-advanced-custom-vfx-system/3867553">VFX Forge</a>.</li>
  <li><strong>BasePart Properties</strong> - Animate <code>CFrame</code>, <code>Color</code>, <code>Size</code>, and <code>Transparency</code> dynamically.</li>
  <li><strong>Screen UI Elements</strong> - Automatically manages and animates <code>ScreenGui</code> elements like Subtitles (<code>TextLabel</code>), Letterboxes (<code>Frame</code>), or Vignettes (<code>ImageLabel</code>).</li>
  <li><strong>Camera Movement</strong> - Create synchronized cutscenes effortlessly with advanced <code>RelativeA</code> and <code>CameraTarget</code> tracking.</li>
</ul>

<hr />

<h2>Installation</h2>
<p><a href="https://github.com/FogoEGelo/FGController-System/tree/main">Download</a> the library and place it in any service accessible by the client. The recommended location is <code>ReplicatedStorage</code>.</p>

<hr />

<h2>Important Observations</h2>
<ul>
  <li>All the Meshes/Parts have to be <strong>anchored to play correctly</strong>, and it is highly recommended to disable <code>CanCollide</code>.</li>
  <li><strong>Do not name anything with just numbers</strong> (e.g., a MeshPart named "1"), because Moon Animator does not save these correctly.</li>
  <li><strong>This module must be used on the client</strong> to handle the camera and UI properly.</li>
  <li><strong>For now</strong>, the module only supports <code>shared.vfx.emit</code> in events with code.</li>
  <li>The codes in events <strong>must be in Code Begin</strong>, not Code End, for the time being.</li>
  <li>To the <strong>local camera</strong> work correctly you have to put the principal part/character of the animation in the position (0, 0, 0) and animate the camera relative to her, after this, send the char/part CFrame to the RelativeA, the zero direction of the camera is equal to the look vector of the character/part.</li>li
</ul>

<hr />

<h2>Core Structure</h2>
<p>The library includes two main ModuleScripts:</p>
<ul>
  <li><strong><code>FGController</code></strong> - The core engine that calculates time, syncs tweens, and runs the animations.</li>
  <li><strong><code>AbilityCreate</code></strong> - A user-friendly wrapper that parses your JSON data, sets up the UIs/Cameras automatically, and plays your abilities using <code>FGController</code>.</li>
</ul>
<blockquote>
  <p><strong>Note:</strong> You do not need to interact with <code>FGController</code> directly. <code>AbilityCreate</code> handles all the heavy lifting and is the primary module you should use.</p>
</blockquote>

<hr />

<h2>Quick Start Guide</h2>

<h3>1. Adding your animation from Moon Animator</h3>
<p>Create a folder inside <code>ReplicatedStorage</code> named <code>Animations</code>.</p>
<p>When you save your animation in Moon Animator, the save file will appear in <code>ServerStorage -&gt; MoonAnimator2Saves</code>. <strong>Copy and paste</strong> this save file into the <code>Animations</code> folder you just created.</p>
<p>This save file is your <strong>AnimationFolder</strong>. You can reference it in your scripts like this:</p>

```luau
local RS = game:GetService("ReplicatedStorage")
local animationFolder = RS.Animations:WaitForChild("YourAnimationSaveName")
```

<h3>2. Generating the Setup Template (The <code>Objs</code> Table)</h3>
<p>Because your animation relies on specific objects (VFX, Parts, UIs), <code>AbilityCreate</code> needs a table linking the Moon Animator tracks to the real instances in your game. We built an automated tool to generate this table for you.</p>
<p>Run this code in the <strong>Command Bar</strong> or a temporary test script:</p>

```luau
local RS = game:GetService("ReplicatedStorage")
local AbilityCreate = require(RS.Modules:WaitForChild("AbilityCreate"))

local AnimationsFolder = RS.Animations:WaitForChild("YourAnimationSaveName") 
local ModelName = "Character" -- The variable name of the character/model in your script

-- This will print a ready-to-use template in your Output (F9)
AbilityCreate.PrintSetupTemplate(AnimationsFolder, ModelName, true) 
```
<p>Check your Output window, copy the generated code, and paste it into your actual LocalScript.</p>

<h3>3. Playing the Animation</h3>
<p>Once you have your <code>Objs</code> table set up, playing the animation requires just one line of code in your LocalScript:</p>

```luau
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

<hr />

<h2>API Reference</h2>

<h3><code>AbilityCreate</code> (Main Module)</h3>
<p>This is the primary module you will use to trigger animations and handle VFX/UI logic.</p>

<table border="1">
  <thead>
    <tr>
      <th>Method</th>
      <th>Parameters</th>
      <th>Returns</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong><code>.Create()</code></strong></td>
      <td><code>AnimationsFolder: StringValue</code><br><code>Objs: {Instance}</code><br><code>FPS: number?</code><br><code>RelativeA: CFrame?</code><br><code>CameraOrigin: (CFrame|Vector3)?</code><br><code>CameraTarget: BasePart?</code></td>
      <td><code>self</code></td>
      <td>Initializes and immediately plays the animation sequence. Binds UIs, VFXs, and Camera automatically. Default FPS is <code>60</code>.</td>
    </tr>
    <tr>
      <td><strong><code>:Stop()</code></strong></td>
      <td>None</td>
      <td><code>self</code></td>
      <td>Instantly cancels all running tweens, stops time loops, clears memory, unbinds the custom Camera, and hides UI elements.</td>
    </tr>
    <tr>
      <td><strong><code>.PrintSetupTemplate()</code></strong></td>
      <td><code>AnimationsFolder: StringValue</code><br><code>ModelName: string</code><br><code>wait: boolean?</code></td>
      <td><code>nil</code></td>
      <td>Prints a formatted Lua table to the Output window. Set <code>wait</code> to <code>true</code> to use <code>:WaitForChild()</code>, or <code>false</code> for <code>:FindFirstChild()</code>.</td>
    </tr>
    <tr>
      <td><strong><code>.getObjsOrder()</code></strong></td>
      <td><code>AnimationsFolder: StringValue</code></td>
      <td><code>{string}</code></td>
      <td>Returns a sequentially ordered table of string names representing the exact object order required by the animation file.</td>
    </tr>
    <tr>
      <td><strong><code>.PlayCharAnimation()</code></strong></td>
      <td><code>Animation: Animation</code><br><code>Char: Model</code></td>
      <td><code>AnimationTrack</code></td>
      <td>Returns an animation track to play your player animation more easily. Just call <code>:Play()</code> on the returned track.</td>
    </tr>
  </tbody>
</table>

<br />

<h3><code>FGController</code> (Internal Engine)</h3>
<p>While <code>AbilityCreate</code> automates this, here is the API for the underlying engine in case you need to build custom, low-level implementations.</p>

<table border="1">
  <thead>
    <tr>
      <th>Method</th>
      <th>Parameters</th>
      <th>Returns</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong><code>.new()</code></strong></td>
      <td><code>object: Instance</code><br><code>fps: number?</code><br><code>cameraOrigin: (CFrame|Vector3)?</code><br><code>cameraTarget: BasePart?</code><br><code>relativeA: CFrame?</code></td>
      <td><code>self</code></td>
      <td>Creates a new controller instance tied to a specific object. Handles specific camera logic if the object is <code>Workspace.CurrentCamera</code>.</td>
    </tr>
    <tr>
      <td><strong><code>:SetAnimation()</code></strong></td>
      <td><code>AnimationFolder: StringValue</code><br><code>Config: Config?</code></td>
      <td><code>self</code></td>
      <td>Binds the animation folder to the object. Accepts an optional config table. <code>LocalPos</code> defaults to true (changes world position to local based on the part's actual CFrame). <code>LocalCamera</code> dictates if camera tracks are relative.</td>
    </tr>
    <tr>
      <td><strong><code>:Play()</code></strong></td>
      <td><code>Loop: boolean</code><br><code>Objs: {Instance}?</code></td>
      <td><code>self</code></td>
      <td>Starts the animation in a parallel thread (<code>task.spawn</code>), utilizing strict <code>os.clock()</code> calculations for perfect frame syncing and event tracking.</td>
    </tr>
    <tr>
      <td><strong><code>:CallBack()</code></strong></td>
      <td><code>CallBack: () -&gt; ()</code></td>
      <td><code>self</code></td>
      <td>Connects a function to fire exactly when the animation finishes naturally or is stopped.</td>
    </tr>
    <tr>
      <td><strong><code>:Stop()</code></strong></td>
      <td>None</td>
      <td><code>self</code></td>
      <td>Cancels all active <code>TweenService</code> tracks for this specific object and halts the loop.</td>
    </tr>
  </tbody>
</table>

<hr />

<h2>Price &amp; License</h2>
<p><strong>License (VFX-DL) Version 1.0</strong></p>
<p>To buy the license to use this module, the price is <strong>$20</strong>. Please send me a message on Discord to purchase it.</p>
<p><em>Read the start of the <code>AbilityCreate</code> or <code>FGController</code> modules to check the full terms of use.</em></p>

<h2>Updates</h2>
<p>This module is updated frequently. If you notice an error, feel free to send me a message or create an issue/comment on the repository! Because these modules are recent, there might be occasional bugs, but I am actively fixing them.</p>
