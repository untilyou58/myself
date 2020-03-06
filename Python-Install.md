1. `sudo apt install software-properties-common` So software-properties-common is what ?
ans:
- Base on described in `apt-show sofware-properties-common` This software provicdes an abstraction of the used apt repositories. It allows you to easily manage your distribution and indepentdent software vendor software sources.
- In practice that means it provides some useful scripts for adding and removing PPAs:
```
$ dpkg -L software-properties-common | grep 'bin/'
/usr/bin/add-apt-repository
/usr/bin/apt-add-repository
```
Plus the DBUS backends to do the same via the Software and Updates GUI.
Without it, you would need to add and remove repositories (such as PPs) manually by editing `/etc/apt/sources.list` and/or any subsidiary files in `/etc/apt/sources.list.d` 

2. `sudo add-apt-repository ppa:deadsnakes/ppa` - Add package python PPAs
3. `sudo apt install update` - Fetches the list of available updates
4. `sudo apt install python3.6` - Install python3.6

Let's work with python 3
`$ python3`


+++++++++++++++++++++++++++++++++++++++++++++++++++++++

virtualenvs - pipenv
Pipenv is a dependency manager for python projects. If you're familiar with Node.js' npm or Ruby's bundler, it is similar in spirit to those tools. While pip can small install Python packages, Pipenv is recommended as it's a higher-level tool that simplifies dependency management for common use cases.

`pip install --user pipenv`

++ Install packages for your project
Pipenv manages dependencies on a per-project basis. To install packages, change into your project's directory (or just an empty directory) and run:
```
$ cd project
$ pipenv install requests
```
Now create one file `main.py` to test pipenv then run 
```
$ pip env python3 main.py
```

+++++++++++++++++++++++++++++++++++++++++++++++++++++

Lower level: virtualenv

virtualenv is a tool to create isolated Python enviroments. virtualenv creates a folder which contains all the necessary executables to use the packages that a Python project would need.
It can be used standalone, in place of Pipenv.

Install: `pip install virtualenv`
Check version: `virtualenv --version`
Use: - Create a virtual enviroment for a project
```
$ cd project_folder
$ virtualenv venv
```
Activated: `source venv/bin/activate`
Install packages: `pip install request`
Deactivated: `deactivate`

+++++++++++++++++++++++++++++++++++++++++++++++++++

Other notes :) 

Running virtualenv with the option `--no-site-packages` will not include the packages that are installed globally. this can be useful for keeping the package list clean in cause it needs to be accessed later. In order to keep your enviroment consistent. it's a good idea to "freeze" the current state of the enviroment packages. To do this, run:
`pip freeze > requirements.txt`
This will create a requirements.txt file, which contains a simple list of all the packages in the current enviroment, and their respective versions. You can see the list of installed packages without the requirements format using `pip list`. Later it will be easier for a different developer (or you, if you need to re-create the environment) to install the same packages using the same versions:
```
$ pip install -r requirements.txt
```
This can be help ensure consistency across installations, across deployments, and across developers.

++++++++++++++++++++++++++++++++++++++++++++++++

Python path :(

Here are important enviroment variables, which can be recognized by Python -
1. PYTHONPATH - It has a role similar to PATH. This variable tells the Python interpreter where to locate the module files imported into a program. it should include the Python source library directory and the directories containing Python source code. PYTHONPATH is sometimes preset by the Python installer.

2. PYTHONSTARTUP - It contain the path of an initialization file containing Python source code. It is executed every time you start the interpreter. It's named as .pythonrc.py in Unix and it contain commands that load utilities or modify PYTHONPATH.

3. PYTHONCASEOK - It is used in Windows to instruct Python to find the first case-sensitive match in an import statement. Set this variable to any value to activate it.

4. PYTHONHOME - It is an alternative module search path. It is usually embedded in the PYTHONSTARTUP or PYTHONPATH directories to make switching module libraries easy.

--------------0----------0-----Pyhton looks for modules in "sys.path"

Python has a simple algorithm fo finding a module with a given name , such as `a_module`. It looks for a a file called  a_module.py in the directories listed in the variable sys.path.
```
>>> import sys
>>> for path in sys.path:
...     print(path)
...
```
The a_module.py file is in the code directory, and this directory is not in the sys.path list.
Because sys.path is just a Python list, like any other, we can make the import work by appending the `code` directory to the list.
```
>>> import sys
>>> sys.path.append('code')
>>> import a_module
```

