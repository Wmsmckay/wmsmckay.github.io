---
title: Minecraft Discord Whitelist Bot
date: 2022-06-10
categories: [discord]
tags: [minecraft, docker, python]     # TAG names should always be lowercase
publish: True
---

Last week I was able to complete a project that I had wanted to do for a few months: figure out how to expose my self-hosted services while sitting behind someone else's network.

If you want to read more about how I exposed my services, particularly my Minecraft server, you can read that post [here](#).

### Discord Whitelisting Bot

Because I have read lots of horror stories about exposing services to the web, I was trying to be very careful with how open I left things. For my Minecraft in particular there were challenges (mostly a special form of laziness we call automation). Minecraft doesn't support password protection, but it does support whitelisting. That means I can put all of my friend's usernames into the whitelist file and they can all connect through my new domain name to continue mining and crafting. But the software engineer in me groaned at having to copy and paste the usernames of all 6 active players into the folder, so I decided to design a Discord bot that will do that for me. 

The way it works is the Discord bot listens for anyone to react to a specific post detailing how to connect to the server. If a user reacts with a ✅ then the bot will check to see if they are already verified. If the user has already been verified then nothing happens. If they have not, then the bot sends the user a DM asking for their username. The user's response is sent in a http get request to Mojang's API to see if the user exists. If the response comes back positive, then the user is given the verified role and they are not allowed to submit any more whitelist requests to the bot. The bot then connects to my Minecraft server through a RCON connection and whitelists the player. 

I've very proud of the solution I found because it allows any of my friends to connect without having to go through any waiting process and they don't have to ask anyone for help. I also Dockerized the bot so if someone else wants to user it, they can! 

The version I am currently running is for special cases because my Minecraft server is in a Docker container. Because of that, I need to `exec` into my container before I execute any commands. In the next iteration, I will have an environment variable set to let the bot know if the server is in a container or not before executing any commands.

[Source code on Github](https://github.com/Wmsmckay/discord-minecraft-whitelist/tree/master)\
[Image on Dockerhub](https://hub.docker.com/repository/registry-1.docker.io/wmsmckay/discord-minecraft-whitelist/general)