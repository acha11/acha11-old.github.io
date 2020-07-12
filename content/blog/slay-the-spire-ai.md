---
title: "Slay the Spire Ai"
date: 2020-07-12T11:30:19+10:00
slug: ""
description: ""
keywords: []
draft: true
tags: []
math: false
toc: false
---

[Slay the Spire](https://store.steampowered.com/app/646570/Slay_the_Spire/) is a single-player computer card game. The aim is to fight through an increasingly difficult series of battles to defeat an end boss. The game is run-based, with successful runs lasting around 60 minutes (although this varies a lot depending on style of play).

Each run consists of many battles. Each battle is fought over multiple turns. During each turn, the player plays cards that attack enemies, defend against enemy attacks, or apply special status effects. Your deck of cards changes during the course of a run as you opt to add, upgrade or remove cards along the way. Your chances of winning the individual battles depend heavily on the contents of your deck.

Said another way: the decisions you make at the strategic ("overworld") level (e.g. Which card should I add to my deck? Should I tackle this optional, difficult battle with valuable but unknown reward?) determine the level of difficulty at the tactical (battle) level.

I've been playing the game on and off for a couple of years, and steadily improving my level of play. But since the start, I've had an urge to code an AI capable of playing the game. This urge often kicks in when the experience of playing a game starts to feel mechanical. Certainly, the tactical battles are often a matter of applying a fairly simple set of heuristics.

Recently, I learnt of [CommunicationMod](https://github.com/ForgottenArbiter/CommunicationMod), a mod for Slay the Spire that allows external code to "play" the game.

I've used that mod to code up a [quick PoC AI](https://github.com/acha11/SlayTheSpireAi) that's capable of brute-forcing through a few initial battles, despite an extremely limited understanding of the game. 

It knows how to use only two of the hundreds of cards in the game. It plays those cards using a very simple heuristic that's far from optimal. It ignores the strategic level of the game entirely (never taking card rewards, always taking the first of multiple paths open to it, never shopping, and always skipping events).

But it is a working PoC, and it might be a useful skeleton if you're looking to do something similar using ForgottenArbiter's mod.

I believe ForgottenArbiter developed this mod in the first place to allow them to implement an AI, which I believe has beaten the game. That'd be a fun goal to work towards.

I suspect that the following roadmap would get us to an AI that could beat the game, as Ironclad, perhaps 5% of the time:

  * Basic AI that can interact with the game (Done)
  * Tactical layer - basic planning and execution
    * Implement a model of tactical state that updates to reflect a player action, and determine which subsequent moves would be permitted. Support only starter deck cards for now, for one class.
    * Implement a heuristic to "score" tactical state to guide AI planning. (For example, lower player HP would be penalised, defeating the last monster in a battle would be strongly rewarded)
    * Implement a planning mechanism that interacts with the tactical state model & scoring heuristic to devise a plan.
    * Implement an "executor" that runs through a plan, issuing commands in sequence. (This might be quite simple, just issuing the first command, accepting the resulting state from the game, and triggering replanning. This is because new information may arrive after executing a command, as some rules are non-deterministic - random cards being discarded, for example).
  * Strategic layer - naive card selection
    * Implement heuristics to score cards based on naive "tier-list"
    * Implement support for shops, act one random events, etc., so that we take cards according to the heuristic above.
  * Extend tactical and strategic layer's understanding of the game:
    * Tactical layer: progressively implement understanding of the effects of more and more cards, status effects (vulnerable, weak, artifact, intangible, etc), relics.
    * Strategic layer: Implement heuristics to better score cards based on current contents of deck, current relics, upcoming battles and boss, etc.
    * Strategic layer: Implement heuristics to score offered relics based on naive "tier-list"
  * Consider extending the tactical lookahead beyond one turn (might not be necessary to get us to a level that can beat the game)
  * Implement understanding of the behaviour of specific monster AIs (e.g. monster X runs on a pattern "attack for 11 then block for 8 then add a Wound card to the player's deck") (might not be necessary)
  * Tactical layer - implement potion use (might not be necessary)

That roadmap omits a lot of strategic & tactical considerations that strong players take into account, but I suspect it'd be enough to get at least one victory in 20 attempts. Let's see.