---
title: "Whatsmore"
summary: A better way to send broadcasts in whatsapp
description: Whatsapp broadcast feature doesn't fit my usecase :\ so I scripted this out
date: 2022-11-14
tags: ["scripts", "whatsapp","automation"]
author: "Aravind S"
url: "/scrapbook/whatsmore"
---

### My usecase
I needed to send a broadcast to a list of numbers from the responses received from a form. Okay! obviously I could've just created a group.. but nope, I wanted people to opt-in whether they wish to be added to our group. But whatsapp just wouldn't let me to do that. Turns out broadcast messages are only sent to those who already have your contact saved. How uncool is that -_- (like every other feature in whatsapp)

Anyway I did some googling and made this 👇 script to automate the process

### Requirements installation
```bash
$ pip install pyautogui
$ pip install pandas
```
Other requirements: A browser with whatsapp desktop logged in

### Recipients file
Create a recipients.csv file with the below format
```csv
MobileNumber,Message
91_9496175003,"YOUR MESSAGE"
```
>Follow the convention for MobileNumber as shown above ie., COUNTRYCODE_PHNUMBER. Also make sure to encose your message in quotes if it includes whitespaces!

### main.py
```python
import pyautogui as pg
import webbrowser as web
import time
import pandas as pd
data = pd.read_csv("recipients.csv")
data_dict = data.to_dict('list')
leads = data_dict['MobileNumber']
messages = data_dict['Message']
combo = zip(leads,messages)
first = True
for recipient,message in combo:
    time.sleep(4)
    web.open("https://web.whatsapp.com/send?phone="+recipient+"&text="+message)
    if first:
        time.sleep(6)
        first=False
    width,height = pg.size()
    pg.click(width/2,height/2)
    time.sleep(8)
    pg.press('enter')
    time.sleep(8)
    pg.hotkey('ctrl', 'w')
```
> Open a blank tab in your default browser before launching the script. This prevents the browser from completely restarting before each iteration.

***NOTE:*** *Use this script at your own discretion. I've tested this script with atleast 200 contacts at one go and have no issues so far. But if you intend to use this script for sending spam messages or any other malicious purpose, whatsapp might suspend your account.*