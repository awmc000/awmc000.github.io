---
title: Solving a Non-Problem with Python
layout: post
post-image: https://i.ytimg.com/vi/VitNzEOxqdk/maxresdefault.jpg
description: Logging into a game with multithreading and mouse and keyboard automation.
tags:
- math
- informative
- technology
---
# The non-problem
My account for an MMO I am playing right now has 
a long password that would be difficult to 
memorize, and I worked through a chapter of my
Python book covering GUI automation with Al 
Sweigart's `pyautogui` module.

I had formed a
habit of copying my password from a file before
I logged into the game... I already had made a
shell alias for running the game with `wine`...
why not write a script to boot up the game, 
enter my login information, and sign in for me?

Identifying information has been stripped from
the program.

# Annotated code

# Annotated code file
```python
# boot_up_wow.py
#
# My password is hard to remember.
import logging, pyautogui, subprocess, threading, time
```

- `logging`: To print information on how the program is running. Helped me identify the need for `threading`.
- `pyautogui`: For sending keyboard and mouse input.
- `subprocess`: For running the game executable through `wine`.
- `threading`: While one thread is running the game and 
waiting for it to exit successfully, the other thread will 
enter login information.
- `time`: The `sleep()` method is used so that the game has 
loaded before input is sent.

```python
logging.basicConfig(level=logging.DEBUG, 
	format=' %(asctime)s -%(levelname)s - %(message)s')

un = 'username'
pw = 'password'

```

Here we configure the debugging log prints and set up the 
strings for username and password.  

```python
def wow_thread_f():
	logging.debug('Opening WOW')
	subprocess.run("wine 'Wow.exe'", shell=True)
	logging.debug('Quit WOW')

```

This function is to be called by the first thread. It opens
the game executable through the `wine` emulator.

```python

def login_thread_f():
	time.sleep(4.5)
	logging.debug('waited 4.5 seconds')

	pyautogui.click(x=900,y=500)
	logging.debug('Clicked the screen')

	pyautogui.write(un + '\t' + pw, interval=0.15)
	logging.debug(f'entered {un}, hit tab, entered {pw}')

	time.sleep(2)
	logging.debug('waited 2 seconds')

	pyautogui.press('enter')
	logging.debug('pressed enter')

```

This function is to be called by the second thread. It 
carries out these steps:

1. Wait a few seconds for the game to fully open.
2. Click the login menu to be sure it is selected.
3. The username field is selected by default, and is also at 
the centre of the screen. Type the username, a tab to switch 
fields, and the password.
4. Wait another two seconds - may be unneccessary.
5. Hit enter to complete logging in.

```python

wow_thread = threading.Thread(target=wow_thread_f)
login_thread = threading.Thread(target=login_thread_f)

wow_thread.start()
login_thread.start()
```

Here we set the threads and then start them!

# Example debug output