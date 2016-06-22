# UE4 Mixed Reality Recording

## Code Integration
This integration is based on **UE 4.12.2**

To integrate the code changes, you need to copy the files from Engine folder to the root folder of your UnrealEngine repo. 

Note that if you have also made changes to the modified files you'll need to merge the changes manually - search for **// MIXED-REALITY** for all the changes I have made.

## BP Integration
There're 2 files in the Content\MixedReality folder that need to be copied to your project:

**RT_VRCapture**: This is the render target which the scene capture actor will render to, you can configure the size of the render target here.

**BP_VRCapture2D**: Put this actor in your level and it'll automatically capture the scene and output the result to RT_VRCapture.

Make sure you tweak the FOV of the capture compoennt 2D on this actor to match the spec of your physical camera used for the recording.

BP_VRCapture2D can be configured to either use pre-defined location (by default), or follow the location and rotation of the 3rd vive controller (enabled by setting **FollowController** to true).

The following logic is implemented in the tick event. The default implementation simply offset the camera from the pawn based on the location and rotation of the controller.
This should work with the default VR setup in UE4 in which the pawn is at the origin of the tracking space and doesn't move in the world.

Note that if your physical camera follows the tracking controller then make sure to apply an offset to the scene capture actor so it matches and actual location of the physical camera.

## INI Setup
The code integration adds a new mirror mode that you need to enable through **DefaultEngine.ini**:

[SteamVR.Settings]
WindowMirrorMode=3

In this mode the output from BP_VRCapture2D is used for rendering the mirror window on the host machine.
