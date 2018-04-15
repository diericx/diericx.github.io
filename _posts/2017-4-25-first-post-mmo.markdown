---
layout: post
title:  "What is this project?"
date:   2017-04-25 18:22:48 -0700
categories: news
project: mmoserver
---

This will be a fairly simple MMO server using GoLang. I'll briefly explain both the server and the client.

Server: All I have is a simple authoritative server that will receive movement data, adjust the entity, and report the game state back to the user.

Client: The client is basically just displaying information. It will receive a game state about 20 times a second and adjust each entity accordingly. Note: Every post will be linked to a specific commit in each repo. I'll link to the source of each commit at the bottom or something. These first commits don't have much though. Like at all, there is really no difference.