# Overview

This is specifically for MDCC but can be used as a general guide

Making a new room in a existing floor has 3 steps:
1. Duplicate and setup files
2. Reposition room and edit details
3. Register and activate room

# Step 1 -- Duplicate and setup files

### 1.1 Duplicate `MDCCnyxCave.tscn` scene

![Step 1.1 - dupe scene](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate01.png)

### 1.2 Rename file

Suggested format is pictured, but do as you like

![Step 1.2 - rename](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate02.png)

### 1.3 Duplicate `MDCCnyxCave.gd` script and rename

![Step 1.3 - dupe and rename](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate03.png)

![Step 1.3 - dupe and rename](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate04.png)

### 1.4 Edit roomID

![Step 1.4 - edit room ID ](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate05.png)

### 1.5 Attach script to scene

![Step 1.5 - attach script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate06.png)

![Step 1.5 - attach script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate07.png)

### 1.6 Edit room details

![Step 1.6 - attach script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep01duplicate08.png)

`self_modulate` - I don't know what this does. Copied from Issix Mod.
`position` - Coordiates of room, relative to floor. Will explain more later.
`roomName` - Name of room as shown in in-game minimap.
`roomID` - Used for referencing room in code. Has to be similar to roomID in Step 1.4
`roomDescription` - Is displayed on the main text screen when inside the room during free-roam.
`roomSprite` - If the room displays an icon on the in-game minimap.
`canWest` and similar - If the room can be entered by walking from that direction. Imagine if a wall exists in that direction.
`roomColor` - Color of tile as shown in the in-game minimap.

# Step 2 -- Reposition room

### 2.1 Open base-game floor scene

![Step 2.1 - open scene](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep02reposition01.png)

![Step 2.1 - open scene](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep02reposition02.png)

### 2.2 Get desired room position

![Step 2.2 - click nearby room](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep02reposition03.png)

![Step 2.2 - move room](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep02reposition04.png)

Undo changes to base-game map

![Step 2.2 - undo changes](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep02reposition05.png)

### 2.3 Edit new room info

![Step 2.3 - open room script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep02reposition06.png)

# Step 3 -- Register and activate room

### 3.1 Create WorldEdit script

![Step 3.1 - create script](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep03register01.png)

Naming suggestion

![Step 3.1 - name](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep03register02.png)

### 3.2 Open reference

![Step 3.2 - references](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep03register03.png)

### 3.3 Rename and edit WorldEdit script

![Step 3.3 - edit](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep03register04.png)

The pictured code creates the room when the game is launched.

Create conditions and If-statements to prevent the room from appearing until needed

### 3.4 Register WorldEdit script in `Module.gd`

IMPORTANT - the room will not appear unless this script is registered in `Module.gd`

![Step 3.4 - register](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep03register05.png)

Done!

![Step 3.4 - preview](https://github.com/Lucci-213/BDCC-editing/blob/master/images/new%20room%20tutorial/roomStep03register05.png)
