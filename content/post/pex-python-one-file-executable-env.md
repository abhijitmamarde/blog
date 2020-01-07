---
date: "2020-01-07"
description: "Awesome tool for quick python executables"
title: "pex - Python Executable"
tags: [ "python", "virtualenv", "tool", "executable" ]
---

# What is `pex`

[pex](https://github.com/pantsbuild/pex) is a smart utility to create the single file independent Python environment, with which you can run Python based programs just like it is a executable program.

# Creating `pex`

to create an environment with `pex`:
```
$ pex requests -o myenv.pex
```

- the list of dependencies here is just `requests` package
- can add more than one dependencies like below:
	```bash
	$ pex requests flask etc -o myenv.pex
	```
- to pin the package to a particular version use:
	```bash
	$ pex 'flask==1.1.1' -o myenv.pex
	# or
	$ pex 'flask<1.1.1' -o myenv.pex
	```
- can create the env with the dependencies as prescribed in requirements file as below:
	```bash
	$ pex -r requirements.txt -o myenv.pex
	```

# Running a `pex`

to run the program with `pex`:
```bash
$ ./myenv.pex hello.py 
```

- this way your program will work as if you have activated the environment, and running your program with python as below:
	```bash
	# after activating the env
	$ python hello.py
	```

Now this would suffice for most of scenarios, but for few, we would need to use/run with some different executable (not python as in this case).

For example, running [flask](http://flask.palletsprojects.com/) applications using `flask` command, or running with [gunicorn](https://gunicorn.org/), [uvicorn](https://www.uvicorn.org/) etc.

In such cases, need to change the `PEX_SCRIPT` as done in below example for running the web app with `gunicorn`.

```bash
$ PEX_SCRIPT=gunicorn ./httpx.pex -w 2 fastapi_demo:app -k uvicorn.workers.UvicornWorker
```

# Upcoming...

Since `pex` essentially makes an one file - "zipped" python environment, there are many more useful ways and scenarios in which this tool would be very helpful.
   
Will add more ways of using this tool as I *explore it more*.


