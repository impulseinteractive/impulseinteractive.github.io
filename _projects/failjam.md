---
layout: page
title: Don't Press The Button
description: A FailJam game
img: https://i.imgur.com/9moFgxN.png
importance: 1
category: game jams
related_publications:
---

Failjam was Impulse Interactive’s first game jam. We gave ourselves one weekend to develop a game where the player must “fail” in order to win.

Before brainstorming ideas, we had to answer some fundamental questions. First off, what does “failing to win” even mean? Is failing really failing if you end up winning the game? Also, how do we take a loss condition and turn it into a winning one? And most importantly, how do we spin that idea into a game that’s actually fun to play?

Traditionally, “losing” in a game means that you’ve been stopped short before reaching your objective. Like losing all your HP while fighting a boss, or running out of time before completing a puzzle. The problem is, losing typically doesn’t feel satisfying. In fact, the frustration, anger, or even excitement after a loss usually fuel players to get back up and continue pursuing victory. So why would we stop the player short of that glorious win by “giving” it to them after a loss? “Oh hello there player, looks like you’ve been slain by the tutorial dummy. Congratulations, you did it!” That doesn’t make for a very satisfying experience, even though it follows Failjam’s theme to a tee.

We pondered these questions, and eventually landed on an idea that would stay true to “winning by losing” while also giving the player a unique and (hopefully) fun experience.

# Making "Failing" Fun?
Failjam’s theme is all about subverting expectations. We want to reward the player for actions that we’d typically punish them for, and we want the player to be surprised by the unexpected reward. Without the surprise factor, the moment will fall flat and ruin the player’s experience.

Let’s use a hack n’ slash game as an example, where the player must dodge and attack to defeat enemies and achieve victory. What if, right on the cusp of victory, you needed to do the exact opposite of what got you so close to the finish line? Imagine slaying enemies through a grueling 30 minute level, only for the final boss to appear, and to defeat it, you need to stand completely still and let it take a swing at your head! No player would let that happen! 

At any other point in the level, standing still would result in a swift death, so the last place you’d stand still is at the very end… And that’s exactly the point! Winning in this way forces the player far beyond their comfort zone, which makes the payoff that much more satisfying.

# The Game: Press The Button
After some brainstorming, we decided to make a platforming game with a core game loop similar to Minecraft parkour. You spawn at the beginning of a level, and your goal is to reach the end of the level by jumping from platform to platform. If you miss a platform and fall into the abyss, you have “failed”, and you promptly respawn back at the start of the level.

Here’s where the “win by failing” comes into play. The end of each level has a platform with a button that says “next level”, but the button is encased in a giant glass box and can’t be pressed. The only way to access the button is by… jumping off the level! When you jump off the ending platform, instead of putting you back at the start, we teleport you inside the glass box, where you can press the button and progress to the next level.

The idea is, when the player first reaches the final platform, they will try to find a direct way to enter the glass. They might try jumping on it, running into it, or even trying to smash it (our game doesn’t have smashing, but it couldn’t hurt to try?) Eventually, they’ll figure out there’s no way to press the button. All that work to get to the end, and the only thing stopping you is a stupid glass box? At this point, there are only two options: Alt+F4, or jump off the edge in frustration. Jumping off the edge is admitting that you can’t progress. That you’d rather *something* happen than just helplessly walking the perimeter of this box. When they eventually jump off, and are respawned in the glass box, they have won by “failing”.

This gimmick continues for several more levels. But in these next levels, this “failure” simply becomes another rule of the game. You get to the end, you jump off the platform, you respawn in the box, you hit the button, and you move on to the next level.

Our final twist comes at the very end of the game, on the final platform of the final level. When they reach this platform, they see something a little different. Firstly, there’s no glass box. The button is completely open and pressable, unlike previous levels. Secondly, the button says RESET GAME instead of NEXT LEVEL. As the player, what would you do if you saw this? Would you press the button, risking to reset all your progress, or would you jump off the level, like you’ve been doing throughout the whole game?

To win the game, the player must… press the button! At this point, players know that jumping off a level’s final platform will help them progress to the next level, so it’s no longer a “failure” action. That button, however, is suspicious. Every previous button has said NEXT LEVEL, and has reliably taken the player to the next level. So it’s reasonable to say that pressing a button that says RESET GAME would send the player all the way to the start of the game, right? So now, this reset button has become the game’s new “failure”! Pressing the reset button will take the player to an overlook, where they can see the entire final level from a bird’s eye view and celebrate their victory.

One loose end, though. What happens if the player jumps off the final level, thinking it will teleport them to the winner’s box? Oh, yeah. We send them back to the start of the game 🙂

# Technical Deep Dive
Now that you understand Failjam’s theme and Press the Button’s premise, let’s dive into the technical aspects of designing and programming our game.

## Level Design

### Aesthetics
When it came to designing the levels for the Failjam, we first developed a vision for the visual-audio theme that was… humiliation. Another reason to add for the player to be “fail-adverse” was the feeling of being on stage, performing for an audience. “According to the National Institute of Mental Health, [stage fright] impacts approximately 73% of the population, making it the most commonly cited fear.” 

While we were certainly not experts in visual design, the goal was to make the levels feel like something out of Squid Game. A dramatic, life or death performance, everything is on the line. To achieve this feeling, we opted for red velvet curtains and harsh lighting that cast stark shadows from every platform. 

As the player progresses, the final level opens up, allowing the player to see the open night sky. All that they have worked for is right there in front of them and the tone shifts to become more ethereal as they rise through the level.

### Layout
The goal of the first level was always to introduce our games mechanics to the player one at a time. The player should need to be able to perform each mechanic in order to progress. In the end, each mechanic besides air-strafing was successfully taught to the player. After a final stretch of sprint-jumps the player would find themselves in front of the button, only to realize it is behind impermeable glass, and are met with the theme of the game. In order to progress, you must fail.

For the final level, the difficulty was turned way up. This would be the final obstacle before the player is presented with the final button, making the choice that much more tense. In particular, the layout of the final level pushes the limits of the player’s jumping ability, maxing out some of the movements vertical and horizontal capabilities. At the very top of the level, the “highest” the player has been, the only way forward to the button is to take the large ramp that slides you not just back to the vertical level of the starting platform, but even lower, encapsulating the overall theme of our game.

## Movement
Our goal for movement was to create a movement system that allowed the player to move around the map with relative ease, as the game required parkour to move around the map. This proved quite challenging as “weightiness” of the character is very important when playing a parkour game. Unfortunately in the short amount of time it was hard to fine tune the movements for press the button. The character movement felt floaty and slidey, which actually ended up working in the case of the game as it made it more frustrating for the player, adding to the overall feel of the game. In the future, fine tuning the movement of the character to get a perfect blend of speed and “weightiness” will make the game more enjoyable to play.

There were many challenges with movement as it has a lot of variables that affect the overall feel that the player has when traversing the map. This will be touched on later. As for the technical side of it, Unreal allows for blendspaces and animation blueprints.

Blendspaces allow for smooth transitions from various animations in UE. This uses the player’s movement speed to determine the correct animation to play. Blendspaces can be used in animation blueprints for idle/walking/running, which is movement speed based. Animation blueprints have various states that the player can be in, whether that be idle/walking/running, swimming, or jumping/falling. This is referred to as locomotion in UE. Using blendspaces, it allows for easy animation changes.

A major issue that was occurring during gameplay was rubber banding when the running animation played. This was caused by Unreal’s “Wrap Input” parameter. After disabling this setting, animations played smoothly and felt fluid. Other small issues were jump height and speed as well as movement speed. Also, allowing for movement in the air, which proved rather challenging to fine tune.

## Spawning & Save States
For FailJam, both player spawning and checkpoints are key to making the success and failure of a level work for the theme of “win by failing”. Conventional expectations for completing a parkour level is progress towards a new checkpoint or level. Failing in a parkour level would result in the player resetting progress to the last checkpoint or even the very start of the level. FailJam follows these conventional rules until key progress is made in the levels where the rules are flipped: falling off will make progress and buttons that would normally progress the level would instead reset progress. These mechanics are controlled by how the game spawns characters when they fall and how progress is saved for tracking level and checkpoint progress.

Player spawning is directed by the gamemode which keeps track of what level the player is in and what the latest checkpoint has been reached. The levels and checkpoints dictate the location the player is spawned in when the player fails a jump and falls into a pit. However, when the game decides to flip the rules, the spawning system needs to keep track of that to change how falling off chooses the spawn because it needs to now make the player progress to a new checkpoint rather than the previous one. Trigger boxes are used to flag the gamemode that significant progress has been made to change the rules of spawning. The flag will tell the gamemode to select a new checkpoint that actually progresses the player on the next time they fall off the map and spawning them at a spot previously thought to be unreachable, flipping the expectation that “falling off means failing”. 

Save states are utilized to make sure progress between game sessions are saved so that the player can load in to where their latest progress was. Typically, the player’s save state will bring the player to the latest checkpoint and level that they have made progress in. However, there is also a scenario with the save state that also embodies the “win by failing” theme. When the player progresses to a new level, the game establishes that interacting with buttons will load the player into a new level. However, there are also other triggers that flip how the player transitions between levels similar to how checkpoint spawning is changed. If there is a trigger box that flips how buttons work, pressing the button will instead spawn the player back to the first level, resetting all of their progress. When this occurs, the save state is also overwritten to the first level so their progress is also pushed back and they have to work their way back up. Instead of “win by failing”, this mechanic is like the inverse “fail by winning”. If the player wants to progress past the trap button, the player needs to fall off at the end when they reach the trigger box that flags the gamemode similar to how the checkpoint rules were flipped.

## Audio
Our goal when designing audio was to make the player feel as closely connected to their movements in game. We took inspiration from Super Mario, which masterfully handles player audio while also injecting personality to make the player feel like they’re playing as Mario or Luigi as they traverse the Mushroom Kingdom. Jumping create a distinct BOING, earning a coin makes a BLING, and jumping down a pipe makes the classic BADUM BADUM BADUM. An immersive soundscape will give players that extra bit of recognition when they perform an action, adding a layer of polish to the overall gameplay experience.

In our game, we wanted to give the player distinct audio feedback to indicate a change of movement state. If they’re walking, play a slow, steady stream of footsteps. Once they start sprinting, increase the frequency and weight of each footstep. If they jump, play a character voice line, like “hup” or “yahoo!” (sound familiar?). Once they land back on the ground, play a thud. These audio queues make gameplay more immersive, but are also critical for the player’s progression through the game. While some jumps can be performed from a standstill, others need to be performed while sprinting, while others are best made while only walking! If we want to create platforming problems for the player to solve, we must also give them the tools to solve them. Precise and reliable audio queues enable the player to quickly recognize when they’re moving, when they’ve become airborne, and once they’ve landed.

On a technical level, audio was unexpectedly challenging to implement. We dove into this project taking audio for granted: Just spend a few minutes recording some noises, hook em up to the movement, and we’re done! However, properly integrating audio into a character’s movement is much more involved than just adding a few hooks to the controls. Tricky interactions between control states can easily break a shoddily–programmed audio system. In simpler terms, does your game make the correct sounds when the player mashes their forehead against their keyboard? To illustrate this problem, here’s the logic flow for playing sprint audio.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="https://i.imgur.com/rtyokPB.jpeg" title="Sprint Audio Logic Flow" class="img-fluid rounded z-depth-1" %}
</div>

We needed to map out every possible interaction between movement states to play the correct audio in each situation. For example, when the player jumps while sprinting or walking, the footstep audio should stop while the player is airbourne, then resume upon landing. Additionally, we should only play the sprint audio if the player is already walking. Otherwise, if they’re standing still, our audio system should ignore the sprint input.

## Decoupling the Audio System
During Fail Jam, we hacked together the audio system such that it was deeply intertwined with the player input system. As a result, every time one dev wanted to touch the audio script, they checked out the entire player blueprint, which blocked other devs from working on other systems within the player class. Looking back, decoupling the audio system from the player class would have saved us time spent waiting for the player blueprint to be unlocked.

After the jam, we decided to extract all the audio script from the player class and create a component, PlayerInputAudioComponent, that we attached to the player class. Every player input, the player class tells the AudioComponent about the updated input state, then asks it to play audio. The AudioComponent reads the current input state and decides to play audio only if it makes sense contextually.

We’ll keep this decoupling technique in our back pocket for future projects. It helps increase development time and improves clarity when systems need to interact with each other.