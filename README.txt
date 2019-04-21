Pavlov VR Character Change Framework 0.2.0 by voxie
Released April 22nd, 2019

This is a modding framework for Pavlov VR maps that enables modders to change the player model 
of the player characters in game.


Disclaimer:
I am in no way officially associated with Pavlov VR or its team. 
This software is in no way officially associated with Pavlov VR or its team. 
Please avoid contacting the Pavlov VR team for support with this software.
If you need support with this software, contact @voxie on the Pavlov VR Discord.


Credits:
Pavlov VR © davevillz Studios Ltd.
Animation Starter Pack courtesy of Epic Games
Everybody in the Pavlov-VR Discord, especially the Workshop Wizards!


Join the Pavlov Discord: https://discord.gg/pavlov-vr


Requirements for a Custom Character Model:
1) The custom character must be rigged and scaled to the Epic skeleton (UE4 Mannequin)
	If it is not rigged to the Epic skeleton, you can try retargeting the skeletal mesh to
	the Epic skeleton. Warning, this often produces suboptimal results, such as incorrect
        bone scale and stretching. 

	You must set Copy Pose as Mesh to true when spawning the BP_CustomCharacterHandler, 
	as Set MasterPoseComponent will not work with a different skeleton. See section below
	on setting up Copy Pose as Mesh.

	See link below for more details on retargeting:
	https://docs.unrealengine.com/en-us/Engine/Animation/RetargetingDifferentSkeletons

	See link below for details on re-rigging:
	https://wiki.unrealengine.com/Exporting_the_Mannequin_Skeleton_from_Unreal_Engine_4_to_Blender_and_Re-importing


Readme:
Quick-start:
0) Migrate this project into your own and copy the CustomCharacter folder into your UGC folder.
   Leave the MovementAnimSetPro folder in the Content folder, this is important!

1) In your definition file, in Compatibility, enable Custom and disable every other mode.
   Check the box that says Custom Game Mode, and give your game mode a custom label.
   (I have not tested that this works in standard game modes.)

2) Drag the Pavlov_GameLogic_CC actor into your level and set the definition file in the Details
   panel.

3) In Enum_CustomCharacters, add an enumerator for each custom character you want to have in your
   game mode.

4) In BP_CustomCharacterHandler, in the function SetCustomCharacterMesh, for each character you 
   have, set the skeletal mesh to your custom character mesh and call any necessary nodes like
   Set Material here.

5) (OPTIONAL BUT NECESSARY IF YOU ARE USING COPY POSE AS MESH)
   a) In BP_CustomCharacterHandler, in the function SetupCopyPoseAsMesh, configure SetAnimInstanceClass
   to use the appropriate AnimBlueprint for your skeletal mesh. Replace the Cast to BP_Mannequin_AnimBP
   with a cast to your AnimBlueprint and copy the wiring.

   b) In your Character's AnimBlueprint, in the Class Settings, implement interface BPI CopyPoseAsMesh Interface.
   Add a SkeletalMeshComponent variable named MeshToCopy, and in the EventGraph, make an Event Set Mesh To Copy
   that sets MeshToCopy to the Event's Pavlov Skel Mesh to Copy output.

   c) In your AnimGraph, put a CopyPoseFromMesh node that takes in MeshToCopy and outputs into a Layered Blend by
   Bone node, which outputs into Final Animation Pose.

   d) In Pavlov_GameLogic_CC, in Spawn Custom Character's Spawn BP_CustomCharacterHandler node, set
      UsesMasterPoseComponent false.

   See CustomCharacter/Characters/UE4_Mannequin/Mesh/BP_Mannequin_AnimBlueprint for an example.

6) In Pavlov_GameLogic_CC, in Spawn Custom Character's Spawn BP_CustomCharacterHandler node, configure options
   PavlovMeshVisibility on spawn, if true, keeps the default mesh on a player spawn.
   If false, the custom character will show on player spawn.
   Set Custom Character Enum to the character you wish to spawn. This can easily be set up to spawn different
   characters per team.

7) Drag a BP_CC_ChangeVolume into the level. In the Details panel, set the CustomCharacterToSwitchTo enum 
   to the custom character you want to switch to when walking into the volume. If you want the volume to 
   hide the custom character and show the Pavlov soldier, set true RestorePavSkelVisibility. If you are 
   debugging and wish to show the custom character and Pavlov soldier at the same time, check true Debug.

8) In BP_CC_ChangeVolume, in the ConstructionScript, set the MakeLiteralText to a name that describes
   your character.

9) (VERY IMPORTANT WILL NOT WORK WITHOUT THIS STEP!)
   Right click your custom character skeletal mesh and assign skeleton as the skeleton in the MovementAnimsetPro 
   folder.

10) Set up Pavlov Spawns and stage or deploy.
    That's it!


Known Bugs / Limitations:
Limitation: The custom character has no collision
    - It could be possible to detect overlap events in the Bullet collision channel to add damage to
      the player pawn when the custom character is shot. As of right now, there is no collision for
      the custom character.

Limitation: Default Spawn and Possess Pawns node is used.
    - When using my own custom spawn and possess logic, I ran into problems where players would spawn but not
      possess their pawns. The default Spawn and Possess Pawns spawn logic is much more reliable.

Limitation: Tags are cleared on player spawn.
    - It might be desired to keep track of player state by use of tags. Currently, all tags are removed on
      player spawn.

Limitation: Currently, character changing is only compatible with Custom game mode.
    - This can be avoided by placing the logic in the gamemode in a separate actor. 

License:
Copyright 2019 voxie

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

This Software may only be used/modified for the express purpose of use in Unreal Engine projects for Pavlov VR.

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.