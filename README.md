# PyMachine
PyMachine is a NodeJS module written in TypeScript, for running MachineLearning scripts (Python scripts) at Anaconda environment.

## Features
+ Run python scripts on Anaconda environment
+ Load heavy python libraries only once
+ Great performance thanks to the one-time initiation.

### Dependencies
This module dependes:
* Anaconda (https://github.com/Anaconda-Platform/anaconda-project) being installed on your machine.
* Python-shell (https://github.com/extrabacon/python-shell) (included on package.json of this module).
* Dotenv (https://github.com/motdotla/dotenv) - not used by default, but PyMachine has an option to use an Environment variable, so it's recommented.

Currently, PyMachine only works on Windows OS.

## Usage
### NodeJS
This is how your JS code shoult look like:
```typescript
import { PyMachine } from "PyMachine"

let pm = new PyMachine('.....path.../script.py', PATH_TO_ANACONDA);

let data = { ...your data... };

// Call run() as many times as you want!
pm.run(data).then((result) => {
  // Do whatever you want with the results
  console.log(result);
});
```
Notice that you dont have to send the Anacoda path for each PyMachine object. You can use an Environmet variable called `PYMACHINE_ANACONDA_PATH`. for example:
```typescript
import * as dotenv from "dotenv";
dotenv.config({path: YOUR_ENV_FILE});
.
.
let pm = new PyMachine('.....path.../script.py');
```
As you can see, now you don't need to send PyMachine the path of Anaconde.

### Python
And here is an example for a python script. this script listenes to `stdin` "forever" and return an answer for each data object.
```python
import sys
import json
from sklearn.neighbors import NearestNeighbors
import numpy as np

while True:
    j = json.loads(sys.stdin.readline())
    X = np.array(j['X'])
    y = np.array(j['y'])
    nbrs = NearestNeighbors(n_neighbors=2, algorithm='ball_tree').fit(X)
    result = nbrs.kneighbors([y],return_distance=False)
    print((''.join(str(e) for e in result)).replace(' ',','))
    sys.stdout.flush()
```

### How it works:
+ PyMachine will run your python script at Anaconda environment only once.
+ It's your responsibility to write the python script so it will listen to stdin forever.
+ Your python script should be able to parse an incoming data on json format (see example_script.py).
+ Your python script should also be able to return it's answers on json format.
+ Now, every time you want to send data to the script, you simply call "your_pymachine_object.run(data)" and you get back a Promise.
+ PyMachine is managing a queue so even if you send many requests to your PyMachine simultaneously, an answer will be returned to the related request.
