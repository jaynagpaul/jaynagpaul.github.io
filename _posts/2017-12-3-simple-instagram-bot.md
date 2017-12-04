---
layout: post
title: Creating A Simple Instagram Bot
description: This tutorial will detail over how to create an Instagram bot to target and follow specific users.
image: assets/images/instagram.jpg
tags: instagram bot python tutorial
comments: true
---

In this blog post we'll be creating a very simple bot for Instagram, this post assumes a basic understanding and working installation of Python 3.6. By the end of this post, we will have a working Instagram Bot which will be able to target and follow users, in order to convert them into followers themselves.

## Getting Started
Let's first get everything setup.
```shell
>>> mkdir instabot
>>> cd instabot
>>> pip install -U git+https://github.com/LevPasha/Instagram-API-python
```

## First Steps
Now we'll import modules we need and ask for anything we need. You can fill these out in the terminal, and press enter when you are finished.
```python
from InstagramAPI import InstagramAPI
import time

def ask_for_info():
    """
    Ask the user running this script for certain info we need.
    Returns a dictionary with the answers.
    """
    username = input('What\'s your Instagram Username? ')
    password = input('What\'s your Instagram Password? ')
    num_to_follow = input('How many people should we follow per day? ')
    tags_to_use = [input('What tag should I use? (No #) ')]
    while True:
        more = input('Any more? (Leave blank for no) ')
        if more == '':
            break
        tags_to_use.append(more)

    # This does not validate the username or password, so make sure you enter in the right one!

    return {
            'username': username,
            'password': password,
            'num_to_follow': int(num_to_follow),
            'tags_to_use': tags_to_use
        }
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
Now we have a logged in Instagram account, it's time to find the targets to follow. We'll search up people who have recently posted a picture using the hashtag(s) you provided before.
```python
def find_targets(api, tag):
    """
    Search for users who have posted a picture with tag, and return a list of user IDs.
    """

    ok = api.tagFeed(tag) # Automatically prints an error
    if not ok:
        raise Exception # Error the script

    resp = api.LastJson
    images = resp['items']

    users = []
    for image in images:
        users.append(
            # We use .get so we can filter out invalid data.
            # Invalid data is rare, but in this case we are prepared
            # pk is ID
            image.get('user', {}).get('pk', '')
        )

    # a list of users from the list of users which id isn't an empty string
    return [u for u in users if u != '']
```

## Following
Great job if you've made it this far! Just to recap, so far we can/have:
1. Imported the required modules
2. Can login
3. Can find targets based off of recent hashtags

Now it's time to follow them, we have to be smart about this as to avoid a ban. So we pause in between follows.

```python
def follow_all(api, targets, time_to_wait):
    """
    Follow all targets, while waiting time_to_wait in seconds
    """
    total_targets = len(targets)
    num = 0
    for t in targets:

        ok = api.follow(t)
        if not ok:
            print('Error while trying to follow ID: %s', t)

        num += 1
        print('Followed %d/%d' % (num, total_targets))
        time.sleep(time_to_wait)
```

## Wrapping it up
Time to bring it all together. We'll ask for your answers, find the targets, and follow each of them.
```python
if __name__ == '__main__':
    answers = ask_for_info()
    print('Thanks!')
    
    username = answers['username']
    password = answers['password']
    num_to_follow = answers['num_to_follow']
    tags_to_use = answers['tags_to_use']

    api = login(username, password)

    targets = []
    for tag in tags_to_use:
        targets += find_targets(api, tag)
    print('%d Targets Acquired!' % len(targets))

    secs_in_a_day = 86400
    time_to_wait = secs_in_a_day / num_to_follow

    print('Beginning to follow!', 'Make sure you keep an eye on your following count.')
    follow_all(api, targets, time_to_wait)
    print('All Done!')
```

Thanks for reading. If you have any more bot ideas you would like me to go over, comment them below. You can get the full script here:
{% gist 43fe6257a999522086283a41fc9a35b4 instabot.py %}

## Credits
- [LevPasha](https://github.com/LevPasha) for writing the InstagramAPI wrapper