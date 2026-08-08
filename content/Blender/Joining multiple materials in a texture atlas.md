## Pick objects
- Multiple objects can share 1 Material, they will all have 1 Material slot but it will still be faster for the GPU to not switch Materials/Shaders/Textures to render them, having more Mesh Renderers just puts more stress on the CPU
## With objects selected, create a new UV Map
1. With all objects you want to affect selected, navigate to the Data tab and select UV Maps
2. Click on the + sign (do not click on the - sign, we need both UV Maps for this)
3. Click on the camera and on the text, the new UV Map should be selected and with a camera for this to work
##  Pick out Materials to join
- For better looks and less work, do not join Materials that are fully Opaque with Transparent Materials
## Atlas the UV Map
1. Current UV Map by default is a copy of the previous one
2. With all the target objects selected, go into Edit mode
3. Press A to select all vertices/edges/faces
4. On the left side of the screen, right click and select Unwrap
- Instead of the previous overlapping UV islands, you'll get neatly packed non-overlapped islands, this means all the target meshes can now use the same texture without looking jumbled up
## Prepare Shaders for Baking
You will need to do this for **each** material on **each** object (I am truly so sorry)

>[!info]
>If Blender does this cursed shit-tier programming thing where it glitches and reduces the FPS of your windows without reducing the FPS of your mouse, press Windows + Ctrl + Shift + B, this resets the Windows graphics subsystem and reinitializes the Desktop Window Manager (DWM) without restarting your computer
>
>If you want to be proactive about it, you can save a few seconds by having Task Manager open (Ctrl + Shift + Escape), on the Process tab with Desktop Windows Manager on the search bar, if the FPS look like a slideshow, kill that process, Windows will shortly restart it anyways and the bug will be gone, this skips the unnecessary Windows graphics subsystem restart

1. Go to the Shading tab at the top and on the sidepanel select Material, select a Material slot to see the Shader Nodes in the associated Material
2. Create a UV Map node (Shift + A > "Search..." (or start typing without selecting anything)> UV Map)
3. Click on the icon and select the old UV Map
4. Connect the UV Map's "UV" purple output the each Texture Image Node's "Vector" purple input
5. Make sure the Shader Node outputting something to the Material Output contains the word BSDF or is just straight up called "Emission", if not copy the configuration to a new Principled BSDF Shader Node and connect it to Material Output's Surface, if some custom Shader Node is responsible for the look on the Viewport, nuke it away 
> [!info] Roughness and Metallic **must** be on 0, otherwise the output will be fully black
6. Create a new Image Texture node, select "New" and select "Transparency" or not according to your texture's needs (does it have any transparent sections?) background color is up to your personal preference, this will be the final image we bake everything into
7. Add a new UV Map node, select the new UV Map and connect it to the new Image Texture node
8. Click on the new Image Texture node, make sure it's selected (white outline), this may sound incredibly fucking stupid but that's how Blender knows which image to bake to (amazing UX, I know)
- After that the current material is ready for baking, you can do it in batch by doing this for several materials (recommended) or you can do it additively, if you choose the latter make sure Clear Image is not selected on the Bake section

## Bake the materials into one texture
With all materials having a Image Texture Node with the new UV Map selected **and** all previous Image Texture nodes having the old UV Map assigned, you can now output everything to one single texture

This is the simplest part of all this
1. Go to the Render tab on the sidepanel
2. On Render Engine (at the very top) select Cycles instead of EEVEE
	- You may want to switch it to GPU for faster operation if you have a good GPU, for that you will need to set up Blender for operating with GPU
3. Navigate down on the panel until you find Bake
4. Change Bake Type to Diffuse if your materials have transparency, if the don't, select Emission
5. If you selected Diffuse, uncheck Direct and Indirect (leaving only Color)
6. Press Bake (big wide button at the top of the Bake section) and wait

## Wrapping up
Now you have one texture with all the texture data **and** your mesh set up to properly pick up that data, you will only need that one image in color for any shader, nothing else

## Source
- Youtube video [How to Bake Textures from One UV Map to Another in Blender (Tutorial)](https://www.youtube.com/watch?v=wsO1eozb1Qk&lc=UgwvuB2tx4WtPo76rFZ4AaABAg.AIj1o9-_kViA_Dtn39T6sj)