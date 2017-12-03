---
layout: post
title: Creating A Simple Instagram Bot
description: This tutorial will detail over how to create an Instagram bot to target and follow specific users.
image: assets/images/instagram.jpg
tags: instagram bot python tutorial
---

In this blog post we'll be creating a very simple bot for Instagram, this post assumes a basic understanding and working installation of Python 3.6. By the end of this post, we will have a working Instagram Bot which will be able to target and follow users, in order to convert them into followers themselves.

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
    Returns List[username, password]
    """
    username = input('What\'s your Instagram Username?')
    password = input('What\'s your Instagram Password?')
    # This does not validate the username or password, so make sure you enter in the right one!
    return username, password
```
## Login to Instagram
Now we can use the username and password we just obtained, to login the bot. This is done by creating the InstagramAPI class, and sending the request to login to Instagram. This function may fail on two major cases:
1. Incorrect Username or Password 
2. You need to verify the login, this can usually by fixed if you open the app and are prompted to verify the login attempt

```python
def login(username, password):
    api = InstagramAPI(username, password) # Instantiate the class
    api.login() # Send a login request
    return api
```
# Targetting
Now we have a logged in Instagram account, it's time to find the targets to follw

## Wrapping it up
```python
if __name__ == '__main__':
    username, password = ask_for_info()
    print('Thanks!')

    api = login(username, password)
    print('Login Successful!')
```