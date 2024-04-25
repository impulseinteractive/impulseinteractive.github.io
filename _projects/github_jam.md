---
layout: page
title: Soul Scale
description: A Game Off 2023 Project
img: https://i.imgur.com/v8xr6BL.png
importance: 2
category: game jams
related_publications:
---

## Enemy Activation System
For the musicians that the player needs to target, we opted to have static placements of “enemies” that are activated periodically. The “activated” state includes a spotlight attached to the Blueprint actor that turns on, assigning them a key sequence that is required to “defeat” them, and UI elements that show that sequence along with the time the player has to “defeat” them. The idea was to have the player feel like a conductor that can look at a specific musician and give them directions in the form of keystrokes that correspond to the notes we want the musician to play.

Initially, the activation system used a purely random selection from the musicians with a static timer for each activated musician. Later, this system would be tuned to a difficulty system that varies the rate at which new musicians activate as well as how long the player has to hit them with the correct input sequence.

There were some constraints that we implemented to make sure the way enemies appeared would feel sufficiently challenging as well as ramp up with the difficulty settings. One was making sure the random selection would ignore currently active musicians, that way the relatively low number of musicians does not have repeated activations that did nothing. 

Another related constraint was having a max number of active musicians at a time. With the difficulty ramp up that was implemented, the game can get to a point where just the number of active musicians made the game impossible which was not what we wanted as the source of difficulty. The challenge should be from how fast the player can input keys for each musician, not how many musicians are active on screen. The max was calculated as:

<div style="text-align: center"> (Total Number of Musicians ÷ 2) + 1
 </div>


 so that if we wanted to expand the levels with more musicians, it was easy to translate without hard coded values. 

Another constraint was having a set pool of key sequences that we pull from when activating a musician. We wanted to make sure the sequence felt like playing a section of a musical scale so the sequences were made specifically to be keys that are next to each other on the keyboard as well as having varying lengths from 3 to 5 notes in length.

## Managing Enemy State
As laid out in the previous section, enemies have two states: active, and inactive. When we activate an enemy, we assign it an expiration timer and a random sequence of notes. However, our job doesn't end there. We must manage each enemy's state until the moment we deactivate it.

There are three ways an enemy can deactivate, listed below.
1. The player shoots an incorrect note into the enemy.
2. The player shoots a correct note into the enemy, completing its sequence.
3. The enemy’s timer runs out.

To properly resolve scenarios 1 and 2, we need to detect if a player has shot a correct or incorrect note for an enemy’s sequence by managing the enemy’s sequence while it’s active. We do this with 2 arrays, both containing note values. The first array stores the enemy’s correct sequence, while the second array stores the player’s correct note streak.

*correctNotesArray*

|Q|W|E|R|T|
|Q|W|E|*EMPTY*|*EMPTY*|



*correctStreakArray*

To check if a player has shot a correct note, we check the current note against the next note in the enemy’s sequence. If the notes match, add a note to the correctStreakArray. Otherwise, we deactivate the enemy and *decrease* the player’s score. If the newly matched note completes the enemy’s sequence, we deactivate the enemy and *increase* the player’s score.

## Collisions
The collision system is Soul Scale’s connective tissue. Since the game loop revolves around shooting projectiles at enemies, all of our game systems receive information from each collision to update their game state. When the player plays a note, there are four possible collision cases. In this section, we’ll summarize the expected behavior for each collision state.

1. Hits active enemy, and was the correct next note in the enemy's sequence.
2. Hits active enemy, and was the incorrect next note in the sequence.

Hitting an active enemy, with either a correct or incorrect note, sends the note to the enemy collision system. The enemy then processes the note and plays 

3. Hits enemy while enemy is inactive
4. Misses enemy

Colliding with an inactive enemy OR with any non-enemy simply triggers the neutral blip sound and destroys the note object entirely.

On a technical level, the collision detection logic all lies in the note object itself. We actually hard-coded the logic because there’s only two relevant categories: enemy, and non-enemy. If we’ve collided with an enemy, we call a specific function within that enemy to resolve the collision. Otherwise, we play the neutral blip noise. After handling the collision logic, the note self-destructs.

Looking back, this technique was a little sloppy. Introducing new types of collisions would have forced us to write more code directly into the note object.  More scalable approach would have been to use an Event Dispatcher. When the note detects a collision, we simply call our Event Dispatcher, which signals to other entities that we’ve collided. Other systems can then resolve the collisions as they see fit. However, considering how few cases we needed to handle, we decided that hard-coding our logic was the fastest and lowest-effort approach.

## Controls
With the goal of having the player feel like a conductor, we wanted the controls to feel like you were playing an instrument as well as conducting your orchestra. To that end, we decided on two major control mechanics: the mouse that would aim your music notes, and the keyboard keys that would play them. We ended up using three rows of 4 keys each to create an analogue for playing the piano. Each of those twelve keyboard keys corresponds to a different note on the chromatic scale, so each input should feel distinct, just as it would on a piano. With these mechanics combined, the player will be looking towards the desired orchestra member and inputting a sequence of notes, hopefully fulfilling the conductor fantasy.

## Projectiles
For the projectiles, we knew we wanted something visually interesting, that also made sense in context of the keys being pressed. We landed on two different models, a one-note and a two-note, with twelve color variants. These variants are actually four colors with three saturation variants each. Each color aligns with a column of the keyboard control scheme, with the intensity of the color denoting which row of that column was pressed. We also made the projectiles glow, which made them stand out in the dim orchestra hall. All of this together creates a visually interesting sequence of notes for every sequence you play, and in general was just fun to play around with.

## Audio
Being a game jam entry all about the musical Chromatic Scale, it was important to ensure the audio remained faithful to the scale itself. For this reason, each successful hit on the orchestra members generates a piano sample from the chromatic scale. This allowed us to generate sequences of adjacent notes, or even write songs into the game using these notes.

However, sometimes the player would hit the incorrect orchestra member or enter an incorrect note in the sequence. In these cases, we wanted to make it very clear that a mistake had been made, so that it wouldn’t get lost in the noise. These failure sounds were manually recorded combinations of piano notes that sounded awful together, and we think they get the point across.

The third scenario on any given note input would be if the player missed entirely, and the note collided with any of the level geometry. This is considered a neutral outcome, so we did not want to confuse the player with a piano sound of any kind. Instead, we generated a range of neutral sounding “blip” noises, so that the player also wouldn’t get tired of hearing the same effect on every miss. This had an unintended side effect of also being very fun to play around with by spamming the walls with notes and hearing the cacophony of blips. Combined with the visuals of the notes, this made for a fun toy that was never part of the initial design.

## UI
The goal for UI in Soul Scale was to be minimalist and intuitive for the player to use. The UI implemented in this game included the main menu, the in-game HUD (which showed the vibe bar, player score and orchestra member failure chats), the prompts above the orchestra members, the high score screen, and the pause menu. The lessons learned from creating the UI for Soul Scale were immense, showing the team what works and what does not work. Some UI items that worked well were the HUD for vibe bar and player score, the main menu, pause screen, and high score screen. Some UI items that did not work so well were the orchestra member failure chats, the settings UI, and the way data was accessed and used within the UI.

Ways of improving the UI is making sure we can completely separate various blueprints in UE. Since many items that the UI needed were found in other blueprints, when changes were made it broke the UI. This can be resolved by using Delegates/Event Dispatchers, which Jandro has provided information on how to incorporate these into future projects. Another issue was the prompts in the top right hand corner from orchestra members when the player failed a note sequence. Most players did not seem to notice it and therefore in the future, game testing should be conducted on friends as to help improve features introduced into a game.