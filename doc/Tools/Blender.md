
# Blender

  

## Usefull shortcuts

  

| Panel | mode | keybindings | what they do |
| --------------- | :-----------: | ------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **All** | | ctrl+space | toggle panel extension in full screen |
| **3d viewport** | **all** | shift+space | Pop up tools panel |
| **3d viewport** | **all** | x,delete | propose to remove selection (**D** to confirm) |
| **3d viewport** | **all** | N | Toggle right panel display |
| **3d viewport** | **all** | T | Toggle left panel display |
| **3d viewport** | **all** | alt+Z | Toggle x-ray |
| **3d viewport** | **all** | H | hide selected object/vertices/edges/faces |
| **3d viewport** | **all** | shift+H | hide everything but the selected object/vertices/edges/faces |
| **3d viewport** | **all** | alt+H | hide everything but the selected object/vertices/edges/faces |
| **3d viewport** | **object/editing** | shift+S | Reposition cursor panel |
| **3d viewport** | **object/editing** | shift+RMB | Reposition cursor |
| **3d viewport** | **object** | . (numpad) | Frame selected = Focus on selected object |
| **3d viewport** | **object** | / (numpad) | Isolation Mode |
| **3d viewport** | **object** | ctrl+L > "Object data"| Clone object (make link): first select objects ending by the one you want to clone ([here]([https://blender.stackexchange.com/questions/86900/how-can-i-link-the-mesh-data-of-several-objects-so-they-all-use-the-same](https://blender.stackexchange.com/questions/86900/how-can-i-link-the-mesh-data-of-several-objects-so-they-all-use-the-same))) |
| **3d viewport** | **object** | ctrl+L | To link, eg. to copy **modifier** on the last select object to the firsts selected |
| **3d viewport** | **object** | 0 (numpad) | Camera view |
| **3d viewport** | **object** | G | **G**rab = move selected object |
| **3d viewport** | **object** | R | **R**otate selected object |
| **3d viewport** | **object** | S | **S**cale selected object |
| **3d viewport** | **object** | [S,G,R]+[Z,Y,X] | **Z,Y or X** = constraint transform to the axis |
| **3d viewport** | **object** | [S,G,R]+shift+[Z,Y,X] | transform excluding axis |
| **3d viewport** | **object** | [S,G,R]+[Z,Y,X]*2 | use **local** axis to constraint transformation |
| **3d viewport** | **object** | alt+[S,G,R] | cancel last transformation |
| **3d viewport** | **object** | hold shift (during S,G,R) | more precise |
| **3d viewport** | **object** | A | Select All |
| **3d viewport** | **object** | alt+A | Deselect All |
| **3d viewport** | **object** | shift+A | Add |
| **3d viewport** | **object** | ctrl+A | Apply (rot, pos, scale) |
| **3d viewport** | **object** | RMB | Show context menu |
| **3d viewport** | **object** | ctrl+J | Join selectedobject |
| **3d viewport** | **object** | ctrl+1-5 | Add modifier subdiviser |
 **3d viewport** | **object** | shift+tab | toggle snap mode |
| **3d viewport** | **object** | M | Move to collection  |
| **3d viewport** | **object** | shift+M | Link to collection |
| **3d viewport** | **object** | ctrl+G | Create a collection |
| **3d viewport** | **object** | ; | Change pivot point |
| **3d viewport** | **edit** | shift+R | Repeat action |
| **3d viewport** | **edit** | ctrl+I | Invert selection |
| **3d viewport** | **edit** | shift+D | Duplicate selected faces/edges/vertex (RMB to duplicate "in place") |
| **3d viewport** | **edit** | ctrl+R | Loop cut |
| **3d viewport** | **edit** | f9 | Show tool panel (eg. Symmetrize panel) |
| **3d viewport** | **edit** | alt+LMB | Select loop |
| **3d viewport** | **edit** | hover + L/click a part + ctrl+L | Select all connected (not hidden vertices) |
| **3d viewport** | **edit** | P > selection | Separate selection |
| **3d viewport** | **edit** | shift+N | Recalculate normal |
| **3d viewport** | **edit** | S+[Y,Z,X]+0 | Align vertices along the perpendiculary axe (see [here]([https://www.youtube.com/watch?v=vsgi2hNVkM4](https://www.youtube.com/watch?v=vsgi2hNVkM4))) eg. `S+Y+0` and my plan is spread out the Z axe =  it will align to the Z axes |
| **3d viewport** | **edit** | shift+E | Change crease |
| **3d viewport** | **edit** | ctrl+E | Edge options (seams,...) |
| **3d viewport** | **sculpting** | ctrl+D | toggle **dyntop** |
| **3d viewport** | **sculpting** | shift+D | increase **dyntop** resolution |
| **3d viewport** | **sculpting** | shift+space | increase **dyntop** resolution |
| **3d viewport** | **sculpting** | ² | brush direction menu (add\|subtract) |
| **3d viewport** | **sculpting** | ctrl+shift+LMB | Create a mask [about mask](https://docs.blender.org/manual/en/latest/sculpt_paint/sculpting/hide_mask.html) |
| **3d viewport** | **sculpting** | alt+M | Clear mask |

[https://blender.stackexchange.com/questions/28466/how-to-cut-and-paste-part-of-a-mesh-to-another-mesh](https://blender.stackexchange.com/questions/28466/how-to-cut-and-paste-part-of-a-mesh-to-another-mesh)

To create the toggle subtract/add brushes mode:

[https://blender.stackexchange.com/questions/15113/is-there-a-shortcut-to-switch-between-add-and-subtract-brushes-in-sculpt-mode?noredirect=1&lq=1](https://blender.stackexchange.com/questions/15113/is-there-a-shortcut-to-switch-between-add-and-subtract-brushes-in-sculpt-mode?noredirect=1&lq=1)

 
some from here : [https://en.wikibooks.org/wiki/Blender_3D:\_HotKeys/3D_View/Sculpt_Mode](https://en.wikibooks.org/wiki/Blender_3D:_HotKeys/3D_View/Sculpt_Mode)

## Usefull tools

| view | what it does | where |
| :----------------: | ------------ | ------------------------------------ |
| **edit** | symmetrize | top panel > Mesh > Symmetrize |
| **sculpting>edit** | dyntopo | dynamically rebuild polygon typology |

## Tutorials

| What | Url |
| :-- | -- |
| Bake normal map | https://www.youtube.com/watch?v=tndUB5b4STI |
| Normal map decals | https://www.youtube.com/watch?v=zwEXUN83LeI |
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzI0OTMzMTgzLC03NDQ5NTAzMDAsLTY0MD
E3MzQ0NywtNzI4MzYzMTc2LC02NTIxMDkxMDgsMTcyMTc1ODQz
MywtOTM1NzE3OTY3LC0xNzc2Mjk2NzE0LDkwODA0NDEzLC0yMD
Q5MjkwNjIzLC0xNTAwOTc5NTA0LC0xOTU1MDQ3NzUxLDE4Njk1
NzkzMjcsLTY5NDcwMTIxNCwtNTE5NDk3NTE2LDExNDU1MTMwMD
MsLTE1NDg4ODIzMjUsLTE5Njg0MDAwMTIsMTM4NTM0MzY1LDEz
NTA2ODQ4OTZdfQ==
-->