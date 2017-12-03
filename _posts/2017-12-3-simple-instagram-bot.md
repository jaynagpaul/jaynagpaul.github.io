---
layout: post
title: Creating A Simple Instagram Bot
description: This tutorial will detail over how to create an Instagram bot to target and follow specific users.
image: assets/images/instagram.jpg
---

In this blog post we'll be creating a very simple bot for Instagram, this post assumes a basic understanding and working installation of Python 3.6

## Getting Started
Let's first get everything setup.
```shell
>>> mkdir instabot
>>> cd instabot
>>> pip install -U git+https://github.com/LevPasha/Instagram-API-python.git
```
## First Steps
Now we'll import the module and ask for anything we need.
```python
from InstagramAPI import InstagramAPI

def ask_for_info():
    """
    Ask the user running this script for their Instagram username and password.
    """
    username = input('What\'s your Instagram Username?')
    password = input('What\'s your Instagram Password?')
    # This does not validate the username or password, so make sure you enter in the right one!
    return username, password
```