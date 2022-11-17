---
title: "Bot says Hi"
summary: A discord.py bot template
description: Get started with building a discord bot in python using this template!
date: 2022-10-28
tags: ["snippets", "discord.py"]
author: "Aravind S"
url: "/scrapbook/discordpy-template"
---

### Snippet

Try saying `!hello`

```python
import os
import discord
import requests
from discord.ext import commands
from dotenv import load_dotenv

intents = discord.Intents.default()
intents.message_content = True

load_dotenv()
DISCORD_TOKEN = os.getenv('TOKEN')
client = commands.Bot(command_prefix='!', intents=intents)

@client.command()
async def hello(ctx):
    await ctx.send(f'Hi **{ctx.message.author}**, thankyou for saying hello to me')

@client.command()
async def inspire(ctx):
    res = requests.get('https://zenquotes.io/api/quotes').json()
    quote_text = res[0]['q']
    await ctx.send(quote_text)

client.run(DISCORD_TOKEN)
```