# Francesca-Chapman-Game-Programming-Learning-Journal

## Entry 1: 5/10/2021

Tutorial used: "Top Down Movement in Unity by Brackeys" - https://www.youtube.com/watch?v=whzomFgjT50

In this tutorial I learned how to make a simple player movement script going in 4 directions with the GetAxis input system. I also learned how to set up sprite animations and their blend trees calling each one via parameters and changing states with transitions. I followed the tutorial with no problems, however I didn't know how to set up the tileset environment as shown in the video before the tutorial started. To resolve this I will find a tutorial on tilesets.


Tutorial used: "Tilemaps in Unity" by Brackeys" - https://www.youtube.com/watch?v=ryISV_nH8qw&t=199s

In this tutorial I learned how the Tile Palette worked and I set up a small scene. I had a problem where the tileset I had made covered the player. I fixed this by creating a new sorting layer and assigning it to the player.

## Entry 2: 12/10/2021

Tutorial used: "Topdown 2D RPG In Unity - 18 More Animations" By Hundred Fires Games https://www.youtube.com/watch?v=Grq2PNsop4o&list=PLLtCXwcEVtulmgxqM_cA8hjIWkSNMWuie&index=18

In this tutorial I familiarised myself with setting up more animations. No problems encountered, I did this tutorial just so I could do the next tutorial, as I attempted that one first but I didn't have the animations I needed.


Tutorial used: "Topdown 2D RPG In Unity - 19 Attacking" By Hundred Fires Games https://www.youtube.com/watch?v=elQFhsUFDK4&list=PLLtCXwcEVtulmgxqM_cA8hjIWkSNMWuie&index=19

In this tutorial I set up some more blend trees and transitions and learned how to call them with a new input. I also learned how to control this new state with booleans and a timer. I came across a problem where I tried to get the player to stop moving whilst attacking using the code from the tutorial, however the movement code was done in a different way to mine, and it didn't work. To fix this, I went back into my code to assess the way it programmed the movement and adapted this to replace the piece of code I copied from the video that wasn't working. Despite the code being different, it still works like how it was shown in the video.

## Entry 3: 19/10/2021

Tutorial used: "Topdown 2D RPG In Unity - 20 Attacking the Enemy" by Hundred Fires Games - https://www.youtube.com/watch?v=sDKJAiP8YUg&t=52s

In this tutorial I learned how to get the attack input to detect if it hits an enemy with the OnTriggerEnter function, and it takes away health, destroying it when it reaches 0. I followed the tutorial with no major problems. As I hadn't followed the previous tutorials on enemies I had to make a basic one myself. I was confused why my player was able to walk through it even when it had a box collider and rigidbody, but I realised I had to give my player a box collider as well. The last part of the tutorial used some scripts from another video, so I didn't use this part, however I will go back and look at those tutorials next.


Tutorial used: "Topdown 2D RPG In Unity - 11 Enemy Animations" by Hundred Fires Games - https://www.youtube.com/watch?v=TXUmuG-JF60

No problems encountered in this tutorial. I followed it mostly just to set up the enemy so I can start making the A.I.

## Entry 4: 26/10/2021

This week I went back into what I have already done to make some adjustments. Upon importing my package with the additions I had made the week before, I noticed that a lot of it was missing, though the package had exported correctly - it may have been because I had edited the same scene, adding a tileset which was not in that scene within the package. To fix this, I had to remake them, which didn't take a lot of time.

Tutorial used: "Topdown 2D RPG In Unity - 04 Idle & Face Correct Direction" by Hundred Fires Games - https://www.youtube.com/watch?v=VsSq_Ispo3Q

I edited the animations for the movement code, as in its current state, when you stopped moving you just went back to the front facing idle, meaning the attack animations for anything other than front facing idle never play so you can only attack when moving. To fix this I found a tutorial earlier in the series I have used in previous weeks and discovered that I needed some more floats to detect the last direction the player moved at. Upon adding these the player was now able to face whatever direction it moved last and as a result could attack in idle when facing these directions.

## Entry 5: 2/11/2021

This week I assessed my attack enemy code. I noticed that you can only attack an enemy when running into it, not when staying still. My first thought was that it was something to do with the collision detection, so I looked at the colliders and collision code first. I first tried changing OnTriggerEnter to OnTriggerStay, so that you don't have to attack as soon as you enter the trigger, however this made the enemy's health go down too quickly after one hit. So, I decided to keep to the OnTriggerEnter, but when attacking enable another collider trigger to detect the hit, and disable it once the attack has been completed. It is still not perfect, but I think this is close to finding a solution.

## Entry 6: 9/11/2021

Tutorial used: "How to Make A Simple HEALTH SYSTEM in Unity" by BMo - https://www.youtube.com/watch?v=vNL4WYgvwd8

In this tutorial I learned how to use colliders appropriately to change variables without them misbehaving in order to create a health system. After implementing it with the other pieces of code I have, it worked fine, but then my enemy object's health was misbehaving, going down more than each attack should deal at one time. I realised this was down to the object having a trigger collider to trigger the player's health system, so I removed the trigger and changed OnTriggerEnter to OnCollisionEnter in the player's health script, and the enemy's health worked like it did before the player health implementation. I also noticed that the player's hurt animation was ending too fast, but that was because I had forgotten to change the sample rate in the animation. After adjusting this, the animation worked as intended.

I had another issue where if the player was in the hurt or death animation, when they shouldn't be able to move, they could still change direction, changing the directional animation in that state. I tried fixing this by adding an if statement in the movement script to check if either animation was playing, so the movement wouldn't be able to be triggered, however this was temperamental and only worked with one animation. I fixed it by having the ability to move depend on a bool, which turns off in the immobile states. I used the same technique to stop the player from being able to deal damage when in these states as well.

Tutorial used: "TOP DOWN SHOOTING in Unity" by Brackeys - https://www.youtube.com/watch?v=LNLVOjbrQj4&t=193s

In this tutorial I learned how to instantiate prefabs in order to make a shooting mechanic. I got the core parts of the tutorial to work, but I had issues trying to adapt it to fit with my project. My biggest issue was trying to get the bullets to change animation depending on the direction it was fired in, as it was a prefab and couldn't access other objects to references in the script. I first tried accessing the bullet's animator from the player, and then I tried making a reference to the player in the bullet's script and then assigning it in the player's script, but neither worked without errors. I managed to get it to work by getting the bullet's x and y position when it is instantiated and compare it to its current x and y position.

I realised however that this method didn't work properly if the position's x and y values had both changed from the original, as one would overwrite the other and the bullet would be facing the wrong way. I fixed this by using the bullet's rigidbody velocity to detect the direction of movement rather than position. I originally had the velocity detection in Awake, but this didn't work as the bullets' velocity would be at 0 at that stage, so I moved it to Update and it works. Very rarely it doesn't face the right direction, I assume this is just a timing issue, not being able to detect the velocity fast enough, but other than that it works nicely.

## Entry 7: 16/11/2021

This week I went back to my attack enemy code to find a better solution. I realised that just having the trigger collider the same position and size as the player was what was making the collisions so specific. So, I removed this collider and added 4 empty game objects under the player and gave them a trigger collider, one for each direction, and made it so that they were only enabled when the attack is called, depending on the direction faced.

This now allows the player to attack from within a small range, rather than running straight into the enemy, however they have to keep running back and forth so that the object exits the collider and enters it again in order to deal the next set of damage, and you have to attack as soon as the enemy enters the trigger, otherwise no damage will be dealt. I know that this is because of the OnTriggerEnter function, so next I will try to find a solution that will allow the player to hit the enemy several times without having to leave the trigger, but only dealing one set amount of damage at a time.

## Entry 8: 23/11/2021

I realised that that the issue I was having with my attack function may be to do with the tutorial's method itself rather than trying to make some simple adjustments. So, I found a different tutorial that wasn't specific to a top down, but I could adapt it for my format.

Tutorial used: "HOW TO MAKE 2D MELEE COMBAT - EASY UNITY TUTORIAL" by Blackthornprod - https://www.youtube.com/watch?v=1QfxdUpVh5I

This tutorial was easy to follow and I had no issues getting the core function to work, and it solved the issue I had with the previous code, however I had some issues getting the animations to play. The new code didn't rely on booleans like the previous one, since the function used the counter only and the animation used a trigger condition. I initially didn't use an animation trigger condition, and kept the bool, but I ended up changing it to the trigger, and then the animations were able to play. For the code in the movement script, I didn't seem to be able to access the animator's trigger condition like I could with the bool, so I found GetCurrentAnimatorStateInfo, which checks the state rather than condition. This made my attack function work just like it did before, but with the problems fixed.

## Entry 9: 30/11/2021

I reviewed my scripts in preparation to submit, and I wanted to try and fix my bullet script's minor error of not facing the right direction rarely when instantiated. After referring to some forums, I realised an even simpler method I didn't think to try. Instead of changing the animation in the bullet script, I put it in the shoot script by getting the animator component of the bullet and setting its floats for each directional shoot input. When I tested it, it worked, with no more of that occasional bug appearing. I am glad I was able to fix it, as now all my scripts flow nicely.
