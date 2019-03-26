Pavlov VR Character Change Framework 0.1.0 by voxie
Released March 25th, 2019

Readme:
Quick-start if you have a pre-existing Character and Character AnimBlueprint: 
0) Migrate this project into your own and copy the CustomCharacter folder into your UGC folder.
1) Set your Character to inherit from BP Custom Character Handler in Class Settings
2) Set your Character AnimBlueprint to implement Blueprint Interface BPI Custom Character Interface
    - Make a new variable in the AnimBlueprint of variable type Skeletal Mesh Component
    - In the EventGraph, Set up "Event Set Pavlov Skel Mesh to Copy" as seen in 
         example DifferentCharacter_AnimBlueprint
    - Set up the AnimGraph like DifferentCharacter_AnimBlueprint
3) Drag actor Pavlov_GameLogic_CC into your level and link the definition variable to your definition
     file.
4) In the Pavlov_GameLogic_CC blueprint EventGraph, set up the CharacterSpawnArray 
    - Each array slot corresponds to a team-id.
    - Use a minimum of two character classes if enabling teams
5) Set up Pavlov Spawn actors in your level.
6) In your definition file, set Compatibility to Custom, enable Custom Game Mode, and add a Custom 
   Game Mode Label of your choosing.

Known Bugs / Limitations:
Bug: Sometimes the characters will not replicate the player pose and will t-pose if everybody
     spawns at once, like in the beginning of a round.
    - The check for this is in the Character State Update Timer, where if the actor's setup variables
      are not set, it will pass the proper values again and set the actor's setup variables again. For
      an unknown reason, this happened often on early rounds and happened less frequently in later
      rounds. A suicide on the part of the player usually fixes this. I added a delay between player
      spawns that will hopefully mitigates this problem, because it often happens on round transitions.

Bug: Sometimes the custom characters will not kill properly on player death. 
    - There are a lot of checks to try and reduce this behavior, including the Character State
      Update Timer, Event CleanUpCustomCharacters, and adding tags to the Player State, Player
      Controller, and Pawn to make sure that at least one of those actors' tags can be checked
      by the Custom Character actors so they can kill when they should. This happens when there are
      many players usually.

Bug: When the player crouches, the model slides backwards and loses sync with the player model
    - This might be because of a shifting component transform that is not taken into account when
      attaching the custom character mesh to the Pavlov skeletal mesh. As a consequence, when the
      player crouches, the model shifts backwards and the armor/helmet/weapons stay in place.

Bug: At the beginning of the game, it is not possible to change your team in the menu without
     either being dead or spamming the button.

Limitation: On round end, all players are killed.
    - This is probably easy to change. On player spawn, there would need to be a check that a BP
      Custom Character actor and BP Custom Character Handler already exist for that player, and to
      not spawn them if this is true.

Limitation: On player killed, all custom character actors associated with that player are 
    destroyed, and multiple actors are being spawned by the server every time a player spawns.
    - Ideally, there would be one actor, that when passing the player state to the actor, would set
      up / spawn any additional actors. The problem I ran into when doing this is that the mesh would
      not replicate its pose, even locally.

Limitation: The Copy Pose from Mesh can distort the custom character model.
    - This is especially evident for models that have dissimilar proportions to the UE4Mannequin model
      or Pavlov Player Model. If it's a small distortion on a few bones, it might be able to be worked
      around by using the Transform Bone node in the Character's AnimBlueprint.

Limitation: Copy Pose from Mesh only works for skeletal meshes rigged to the Epic skeleton:
    - This might be able to be worked around with Animation retargeting, but I was not able to figure
      this out in my own tests.

Limitation: The custom character has no collision
    - It could be possible to detect overlap events in the Bullet collision channel to add damage to
      the player pawn when the custom character is shot. As of right now, there is no collision for
      the custom character.


 

Credits:
Animation Starter Pack courtesy of Epic Games
Pavlov VR by davevillz Studios Ltd.
Everybody in the Pavlov-VR discord, especially the Workshop Wizards!


Links:
Pavlov Discord: https://discord.gg/pavlov-vr


License:
Copyright 2019 voxie

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

This Software may only be used/modified for the express purpose of use in Unreal Engine projects for Pavlov VR.

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.