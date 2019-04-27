PyMachine is a NodeJS module written in TypeScript, for running MachineLearning scripts (Python scripts) at Anaconda environment.
PyMachine allows running scripts withoud the need to load Anaconda environment and the python libraries it's including- on every call.

This module dependes:
* Anaconda (https://github.com/Anaconda-Platform/anaconda-project)
* Python-shell (https://github.com/extrabacon/python-shell)

Currently, PyMachine only works on Windows OS.

How it works:
PyMachine will run your python script at Anaconda environment only once.
It's your responsibility to write the python script so it will listen to stdin forever.
Your python script should be able to parse an incoming data on json format (see example_script.py).
Your python script should also be able to return it's answers on json format.
Now, every time you want to send data to the script, you simply call "your_pymachine_object.run(data)" and you get back a Promise.
PyMachine is managing a queue so even if you send many requests to your PyMachine simultaneously, your Promise will return an answer to the related request.
