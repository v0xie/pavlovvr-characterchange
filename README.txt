Pavlov VR Character Change Framework 0.3.3 by voxie
Released August 26th, 2019
https://github.com/v0xie/pavlovvr-characterchange

This is a modding framework for Pavlov VR maps that enables modders to change the player model 
of the player characters in game.

Disclaimer:
I am in no way officially associated with Pavlov VR or its team. 
This software is in no way officially associated with Pavlov VR or its team. 
Please avoid contacting the Pavlov VR team for support with this software.
If you need support with this software, contact @voxie on the Pavlov VR Discord.

If you use this software, please leave a link to the GitHub page on your Workshop page.
(This is optional. The more mappers who use this the better!)

Credits:
Pavlov VR � davevillz Studios Ltd.
Animation Starter Pack courtesy of Epic Games
Everybody in the Pavlov-VR Discord, especially the Workshop Wizards!


Join the Pavlov Discord: https://discord.gg/pavlov-vr


Changes in v.0.3.3:
- Fixed missing execution pin in CheckIfMeshIsArmor in BP_CustomCharacterHandler
- Disable debug text by default in BP_CustomCharacterHandler

Changes in v.0.3.2:
- Fix for helmet/vest meshes not hiding online

Changes in v.0.3.1:
- Simplified logic for attaching to pawn
- Added CC_CleanupInvalidCharacters function to BP_CharacterManager 
    (on timer by default, callable, runs every 60s)
- Performance optimizations
- Characters now use Copy Pose as Mesh instead of Master Pose Component
- Added experimental show/hide custom character body/hands in S_CharacterHandlerConfig (off by default)
- Added debug log to BP_CustomCharacterHandler, toggleable with Debug|bShowDebugText (off by default)
- Upgrade workshop plugin to v.2.1

------------------------------------------------------------------------------------------------
IMPORTANT: Requirements for a Custom Character Model
  The custom character MUST be rigged and scaled to the Epic skeleton (UE4 Mannequin)
        If your model is not rigged to the Epic skeleton or scaled incorrectly, you're most
        likely going to get incorrect bone scale and stretching. The best solution in this
	case would be to re-rig the model in the 3D package of your choice.

	See link below for details on re-rigging in Blender:
	https://wiki.unrealengine.com/Exporting_the_Mannequin_Skeleton_from_Unreal_Engine_4_to_Blender_and_Re-importing

	If your model is not rigged to the Epic skeleton at all (e.g. Mixamo models), you can
        try retargeting the skeletal mesh to the Epic skeleton. The majority of the time this
        will not produce the results you desire.

	See link below for more details on retargeting:
	https://docs.unrealengine.com/en-us/Engine/Animation/RetargetingDifferentSkeletons


------------------------------------------------------------------------------------------------
Readme:
------------------------------------------------------------------------------------------------
Initial Setup:

Recommended migration method (more reliable):
0a) In this project, rename the UGC folder to match your UGC folder's name. Fix up
    redirectors of Content folder, then migrate the CustomCharacter folder into your project.

Alternative migration method:
0b) Migrate this project into your own and copy the CustomCharacter folder into your UGC folder.
   Leave the MovementAnimSetPro folder where it is in the Content folder, this is critical!


------------------------------------------------------------------------------------------------
Character Setup:

1) In CustomCharacter/Data in Enum_CustomCharacters, add an enum for the new character. Set the
   Display Name to a short descriptive name. You will be using this name for the next step.

2) In CustomCharacter/Data in DT_Characters, add a row and set the Row Name to the name you
   used in the previous step. Very important: if the names do not match exactly, you will get
   the default mannequin instead of your character. 

3) In the Row Value settings, set the Name to the name you want your character to show as
   in the CharacterChangeVolume. Set the Mesh value to your skeletal mesh. 
   (To test if this step has been successful, drag a CharacterChangeVolume
   into your level and set the Character value to your new character. If successful, your
   character will show up with the name you set. If not, the default mannequin will show up and
   the name will say Invalid)

4) Right-click your character's skeletal mesh and assign to it the skeleton in the MovementAnimsetPro
   folder. Make sure you re-save the skeleton in the MovementAnimsetPro folder after it prompts
   you. (This step should be optional if your skeleton is already rigged to the default Mannequin
   skeleton, but do it anyway. To check if this step was successful, open 
   CustomCharacter/Characters/BP_Mannequin_AnimBlueprint and see if you can set your character
   mesh as the Preview Mesh in the Preview Scene Settings. If not, something has gone terribly wrong.

------------------------------------------------------------------------------------------------
Advanced Character Setup:
With version 0.3.1, the Character Change Framework now uses Copy Pose as Mesh. 

What this means is that your character's Skeleton asset does not have to strictly be assigned
to the MovementAnimsetPro Skeleton. The only bones that will be copied from the Pavlov Skeleton are
those that have the same name as the bones on your character's Skeleton.

Eye/Jaw bones:
Pavlov VR's August update brought new character models with eyes that track the player and jaw bones.
that move when the player speaks. If you are willing to invest the time in rigging your model to support 
these new features, DM @voxie on Discord and I can walk you through the steps to add these bones
to your character.

------------------------------------------------------------------------------------------------
Game Mode Config:

For a custom game mode:
  1) In your definition file, in Compatibility, enable Custom and disable every other mode.
     Check the box that says Custom Game Mode, and give your game mode a custom label.
     (I have not tested that this works in standard game modes.)

  2) Add a CustomCharacterManager actor to your level.

  3) In your GameLogic actor, add a soft reference to the CustomCharacterManager actor you
     just added to your level. In Event On Player Spawned, resolve the soft reference and do an
     interface call to the resolved soft reference CustomCharacterManager actor. See
     CustomCharacter_GameLogicExample for an example.

  4) In your CustomCharacterManager actor, set your desired configuration in the
     CustomCharacterConfig variable. (See below for more details) See map CustomCharacterDemo in
     the Levels folder for an example.


For a regular game mode (DM, TDM, etc.):
  1) Drag a RegularGamemodeCompatibilityVolume over your spawns. If you have a bunch of spawns in
     close proximity, you can drag one volume over the spawns and scale it to cover them.

  2) Configure the CustomCharacterConfig variable to your liking. See map RegularGamemodeTest in
     the Levels folder for an example.
------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------------
F_CustomCharacterConfig Details:

  RandomCharacterOnSpawn  - If true, will choose a random character for each player
  DefaultCharacterNoTeam  - Default character for every player
  TeamedCharacterOnSpawn  - If true, will choose a character based on their team
    			      Warning: overrides RandomCharacterOnSpawn
  DefaultTeam0_Character  - Default character for team0
  DefaultTeam1_Character  - Default character for team1
------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------------
Known Bugs / Limitations:

Bug: When joining an in-progress game, sometimes the character will be t-posed and not following
     the player. I added a delay to help ameliorate this issue but I have not had the opportunity
     to test it thoroughly.

Limitation: Armor/helmet is hidden. This is usually good since most of the time the position of
            the armor/helmet is off. If your character is wearing a helmet and you get dinked,
            a helmet will still fly off your head.

Limitation: Gore meshes show through custom character.

------------------------------------------------------------------------------------------------

License:
Copyright 2019 voxie

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

This Software may only be used/modified for the express purpose of use in Unreal Engine projects for Pavlov VR.

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.