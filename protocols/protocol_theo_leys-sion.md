Group : Za π

Date : June 2025


# Protocol Specification for ZAPPY AI Client 


## Table of contents

1. [Introduction](#introduction)

2. [Encrypt communication](#encrypt-communication)

3. [Broadcast text messages](#broadcast-text-messages)


# Introduction 

This document is the specification of the protocol we used for the Zappy AI client and gives the significations for every broadcast text used in game. 
 
The Zappy project is a multiplayer network-based strategy game where several teams of players compete in a dynamic world filled with resources and challenges. Each player controls a small autonomous agent (or drone) that can move around, collect food and rare stones, and communicate with teammates using coded messages. The world is a flat, seamless map—when a player walks off one edge, they reappear on the opposite side. 

The main goal is to reach the highest level of evolution by performing elevation rituals, which require specific resources and teamwork. To survive, players must gather food, as each unit of food grants a limited lifespan. Strategic coordination is essential: players must explore their surroundings, manage their inventory, and group together at the right moment to level up. 

Victory is achieved when a team successfully evolves at least six players to level 8. This requires planning, efficient communication, and smart use of the environment.


# Encrypt communication

For broadcasting, a Trantorian communicates to his team with this template message : 

“Broadcast *key* **textMessage**\n” 

The player encrypts **TextMessage** using a XOR cipher with a shared *key*.


# Broadcast text messages


| Symbol     | Description                                                  | 
|:-----------|:-------------------------------------------------------------|
| L  (int)   | Level                                                        | 
| n  (int)   | Quantity                                                     | 
| r  (char)  | Resource = can be food or any other resources defined below  | 
| f  (char)  | Food                                                         | 
| l  (char)  | Linemate                                                     | 
| d  (char)  | deraumere                                                    | 
| s  (char)  | sibur                                                        | 
| m  (char)  | mendiane                                                     | 
| p  (char)  | phiras                                                       |


| Message Broadcast                | Description                               | Type                | Example                             |
|:---------------------------------|:------------------------------------------|:--------------------|:------------------------------------|
| teamLvl                          | Everybody gives its level                 | Question            | teamLvl                             |
| lvl L                            | Gives its level                           | Answer of teamLvl   | lvl 7                               |
| eggNd                            | Needs an egg                              | Question            | eggNd                               |
| eggLd                            | Egg laid                                  | Answer of eggNd     | eggLd                               |
| teamInv L                        | Everybody at level L gives its inventory  | Question            | teamInv 8                           |
| inv,n f,n l,n d,n s,n m,n p,n t  | Gives its inventory                       | Answer of teamInv L | inv,10 f,3 l,28 d,17 s,7 m,6 p,6 t  |
| resNd n r                        | Needs n ressources r                      | Question            | resNd 13 m                          |
| resTk r                          | Ressource r taken                         | Answer of resNd n r | resTk d                             |
| teamMt L                         | Everybody at level L has to come          | Question            | teamMt 6                            |
| arr                              | I arrived                                 | Answer of teamMt L  | arr                                 |
| iSeeU                            | I see u                                   | Answer of teamMt L  | iSeeU                               |
| stop L                           | Nobody in lvl L moves                     | State               | stop 3                              |
