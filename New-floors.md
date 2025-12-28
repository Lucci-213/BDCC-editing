# Overview

I am assuming you have: 
- Godot version 3.6
- BDCC version 0.1.12fix4
- Know how to make a Module

Making a new floor has 4 steps:
1. Make a root floor
2. Add your first room
3. Add more rooms
4. Create an entrance/exit



# Step 1 -- Make a root floor

### 1.1 Create a scene

![Step 1.1 - create a .tscn](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep1rootFloor01.png)

### 1.2 Confirm scene properties

![Step 1.2 - confirm .tscn properties](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep1rootFloor02.png)

### 1.3 Add script to scene

![Step 1.3 - add SubWorld script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep1rootFloor03.png)

### 1.4 Confirm script

![Step 1.4 - confirm SubWorld script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep1rootFloor04.png)



# Step 2 -- Add your first room

### 2.1 Add child scene

![Step 2.1 - add child scene](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep2roomFirst01.png)

### 2.2 Choose template

![Step 2.2 - choose GameRoom template](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep2roomFirst02.png)

### 2.3 Preview map and rename `Room ID`

![Step 2.3 - preview map and confirm Room ID is unique](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep2roomFirst03.png)



# Step 3 -- Add more rooms

### 3.1 Copy first room

![Step 3.1 - copy first room](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep3roomMore01.png)

### 3.2 Paste to root floor

![Step 3.2 - paste first room in root floor](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep3roomMore02.png)

### 3.3 Check scene structure and rename `Room ID` to be unique

![Step 3.3 - verify scene structure and rename Room ID to be unique](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep3roomMore03.png)

### 3.4 Move room to position

![Step 3.4 - move room to position](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep3roomMore04.png)

### 3.5 Repeat until you get the floor shape you want

![Step 3.5 - Repeat](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep3roomMore05.png)


# Step 4 -- Create an entrance/exit

### 4.1 Make a `Module` with 2 `Events`

![Step 4.1 - module folder structure preview](https://github.com/Lucci-213/BDCC-editing/blob/master/images/floor/floorStep4enterExit01.png)

`Module.gd`
```gdscript
extends Module

func _init():
	id = "NewFloor"
	author = "Lucci-213"
	
	events = [
		"res://Modules/NewFloor/Events/NewFloorEnter.gd",
		"res://Modules/NewFloor/Events/NewFloorLeave.gd",
	]
```

### 4.2 Create an entrace

`NewFloorEnter.gd`
```gdscript
extends EventBase

func _init():
	id = "newFloorEnter"

func registerTriggers(es):
	es.addTrigger(self, Trigger.EnteringRoom, "hall_elevator")	

func run(_triggerID, _args):
	addButton("New Floor Button", "Press the button", "enterNewFloor")

func getPriority():
	return 0

func onButton(_method, _args):
	if(_method == "enterNewFloor"):
		GM.pc.setLocation("newFloor01")
		GM.main.reRun()
```

### 4.3 Create an exit

`NewFloorExit.gd`
```gdscript
extends EventBase

func _init():
	id = "newFloorExit"

func registerTriggers(es):
	es.addTrigger(self, Trigger.EnteringRoom, "newFloor01")	

func run(_triggerID, _args):
	addButton("Go back", "Return to the prison", "exitNewFloor")

func getPriority():
	return 0

func onButton(_method, _args):
	if(_method == "exitNewFloor"):
		GM.pc.setLocation("hall_elevator")
		GM.main.reRun()
```

# Final notes

The floor/rooms you have created does nothing.
It is a simply a space that can be explored in-game.
Use `Events` and `Scenes` to populate the space. 






































# OLD

# THIS INFORMATION IS OUTDATED SINCE 0.1.6 BUT YOU CAN STILL FOLLOW THROUGH

# Floors Basics

Floors can be registered by putting the script and scene file in `res://Game/World/Floors`, the ID will be whatever the file name is without the extension.  
Or it can be registered with a module calling a method `registerMapFloor(id:string, path:string)` in `GlobalRegistry`

If the ID existed already, it will replace the floor entirely with new one.  
Registering new floor requires scene file (will go into detail next section)  
[For example:](https://github.com/CanInBad/grindingFloor/blob/d5c395d65413a8da80c5d1e37ec61e0e4566198f/Module.gd#L24)
```gdscript
GlobalRegistry.registerMapFloor("grindingFloor","res://Modules/Z_IgrindingFloor/Floor/grindingFloor.tscn")
```
This will register a floor with ID "grindingFloor" with the path `res://Modules/Z_IgrindingFloor/Floor/grindingFloor.tscn`

# Floor & Room Anatomy
<sup>aka scene file</sup>

A floor is essentially 2 files, the scripting part and the floor layout (or scene part).  
The scripting part is optional but this article will include it.  
It is recommended to include scripting, you will thank me later.

Inside the scene file, a floor is essentially a Node2D which instanced `res://Game/World/SubWorld.tscn` (will go into detail how to create such)

Each floor is consists of rooms in 64 unit grids in both X and Y axis, if they're unaligned, on runtime it will automatically snap to the nearest point in a grid\*

*\* it's not exactly the nearest point, please check [these lines.](https://github.com/Alexofp/BDCC/blob/58885806c1bb8254ce250a56a08d241d5a106623/Game/World/World.gd#L169-L170)*

![Editor showcasing a floor with grid turned on](images/floor/mapEditing1.png)
<div align="center">
   <sup>Editor showcasing a floor with grid turned on, added to make this article prettier :+1:</sup>
</div>

A room is a Node2D which instanced `res://Game/World/GameRoom.tscn` (will go into detail how to create such)

Each room must be assigned to **unique** ID, if 2 rooms have the same ID, whichever loads last get rejected, this also applies to other floors. This means that if you have a room that have the same ID as the other, the room that load last will disappear or crash the game entirely  
(as of 2024-05-16 - I, CanInBad, didn't check if it cause crashes or not)

A room can be configured a lot to do a lot of things, below image is an example
<div align="center">

   <img src="images/floor/roomProperties.png" alt="A image showing properties in a room" width="35%"/>

   <sup>A image showing properties in a room</sup>
</div>
Here are notable properties

1. Room Name
   * This is used to display room's name on the top of the floor preview on runtime  
   Can be repeated with other rooms'
2. Room Id
   * :warning: This is required and must not be repeated.
3. Room Description
   * This is for showing message when you're in a room. The message will appear on the text log.
4. Can X
   * If the direction is off, any room that connected in that direction will not be walkable, This will also affect path finding for scene using them.
5. Room Sprite & Color
   * Changes room's appearance on presets, experiment and see what they do.
6. Grid Color
   * Changes striped color across the room, if its on white the stripe will be invisible.
7. Loctags
   * Changes which type(s) of guards spawns in the room. *I haven't check if having multiple of them do anything.*
8. Population
   * Changes which type of NPC to spawn. Please note that nurses, guards, and engineers are grouped as Guards
9. Lootable and its settings
   * Table Id
     * Which loot table to use when looting.  
       *At the time of writing, I haven't experiment with this yet -CIB* 
   * Around Message
     * What message to display when the room is lootable
   * Credits
     * How much credit do player get once looting
   * Every X Days
     * When do the loots get refills. If not specified, it will never get refilled.  
  
It is recommended that you do not use Lootable properties, using events will give you more flexibility.


# Making A Floor

## Root Floor

First step is to create a new scene file. It can be in the folder we discuss in [Floors Basics](#floors-basics) but this guide will follow registering the floor with module.  
(if you don't know anything about module, you can learn [about it here](What-are-modules))

Right click on FileSystem window, it can be any folder or even a file and click "New Scene..."  
You can name it whatever, the file extension will be `.tscn`.

<div align="center">
   <img src="images/floor/newSceneFloor.png" alt="A image showing steps to create new scene" width="60%"/>

   <sup>A image showing steps to create new scene</sup>
</div>

Next, Click the chain link icon in the scene tab

<div align="center">
   <img src="images/floor/instanceChildScene.png" alt="A image showing mouse cursor hovering over the icon with text &#34;Instance Child Scene&#34;" width="60%"/>

   <sup>A image showing mouse cursor hovering over the icon with text &#34;Instance Child Scene&#34;</sup>
</div>

You then have to navigate yourself to `res://Game/World`, select `SubWorld.tscn`, and open it 

<div align="center">
   <img src="images/floor/floorSelectingSubWorld.png" alt="A image showing a window title &#34;Open Base Scene&#34; while selecting &#34;SubWorld.tscn&#34; in its file explorer" width="60%"/>

   <sup>A image showing a window title &#34;Open Base Scene&#34; while selecting &#34;SubWorld.tscn&#34; in its file explorer</sup>
</div>

After opening, you will see a item/Node2D in Scene tab.  
You can rename it whatever but ideally I recommend name it to the ID you're going to use, mine is going to be "helloWorld".  
*Please note that this Node2D is a root of a floor,* ***without it*** *a floor doesn't exist*

<div align="center">
   <img src="images/floor/floorRenamingRootNode.png" alt="Renaming a node by right click on it and click &#34;Rename&#34;, or click F2" width="60%"/>

   <sup>Renaming a node by right click on it and click &#34;Rename&#34;, or hit F2</sup>
</div>

Adding scripting is easy as selecting the Node2D and click arrow drop down menu then click Extend Script, it is recommended to keep them together in the same folder as the floor scene file.  
A script can do things that events could but bundled with the floor itself.  
*(There will be demo later after we finish making rooms)*

It is recommended to save at this stage. If you haven't save earlier it will prompt you to choose a location. Please be mindful of where you put it.

<div align="center">
   <img src="images/floor/floorExtendScript.png" alt="Image showing steps to extend script" width="40%"/>

   <sup>Image showing steps to extend script</sup>
</div>

## Rooms

After creating floor root node, we'll be covering how to make rooms

### Making Rooms

Firstly, click the root node to select, then click the chain icon again to add another PackedScene, then search for "GameRoom". Select the one that says `Game/World/GameRoom.tscn` then open it.

<div align="center">
   <img src="images/floor/floorAddNewRoom.png" alt="Image showing a dialog box with text &#34;GameRoom&#34;" width="60%"/>

   <sup>Image showing a dialog box with text &#34;GameRoom&#34;</sup>
</div>

Congratulations! You successfully created a room. Please give them ID an make sure that its not duplicating others.

> [!NOTE]
> If you don't put in any ID, it will use the editor's room name or node's name to be used as ID.  
> And if you don't put any room's name, it will use the ID as room's name.  
> [See the relevant code here.](https://github.com/Alexofp/BDCC/blob/58885806c1bb8254ce250a56a08d241d5a106623/Game/World/GameRoom.gd#L83-L86)

If you don't want to click the chain icon everytime you want to add a room, I recommend duplicating it, this can be done by either right clicking a room on the left and click "Duplicate". Or hitting Control + D. Be sure to change the ID every time you do this.

Now to move it.
It is recommend to set up grid snapping for easier room moving. On the scene editor, there is a 3 vertical dot <sup><sub>*(its called kabab menu)*</sub></sup>
next to magnet on a grid, click the kebab menu. Then click "Configure Snap...", a window will pop up. Set the "Grid Step" for both X and y to 64px.

Make sure that you have Grid Snapping turned on when you're moving or else it won't snap

<div align="center">
   <img src="images/floor/floorConfigureSnap.png" alt="Image showing steps to configure snap" width="40%"/>

   <sup>Image showing steps to configure snap</sup>

   <img src="images/floor/floorSnapConfig.png" alt="This is my snap config when editing floor -CIB" width="40%"/>

   <sup><i>This is my snap config when editing floor -CIB</i></sup>
</div>


Room connections is done in runtime so you don't have to manually put them down.  
They are made when a room has walkable room either to east or south. [Please see the relevant code here.](https://github.com/Alexofp/BDCC/blob/58885806c1bb8254ce250a56a08d241d5a106623/Game/World/World.gd#L105-L136)

### Scripting

This section will be short and not very helpful since most things that room scripting can do, events does it better.  
This will take advantage of [signals](https://docs.godotengine.org/en/3.5/tutorials/scripting/gdscript/gdscript_basics.html#signals), and this section will assumed that you already read [Connecting a signal in the editor.](https://docs.godotengine.org/en/3.5/getting_started/step_by_step/signals.html#connecting-a-signal-in-the-editor)

Signal `onEnter` will trigger when a player entered the room  

Signal `onPreEnter` will trigger when a player is moving to the room

Signal `onReact` is only used with buttons.

List of methods you can use is inside `res://Game/World/GameRoom.gd` and examples for using it can be look for inside `res://Game/World/Floors`.

# Example project

This is a example project demonstrating how to add new floor as a module as well as other topics in this page. If you do not know what is a module please consult [this wiki page on Alexofp/BDCC](What-are-modules).

[Here is a link to the project](https://github.com/CanInBad/newFloorExample/)
