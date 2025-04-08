# Using `pyenv` and `virtualenv` to Create Isolated Python Environments

This guide walks you through setting up a Python virtual environment with a specific Python version using `pyenv` and `virtualenv`.

1. Open your terminal and check python version
	  `python3 --version` or `python --version` depending on your system
2. If the python version you have on your system is not the one you want, you can install the library `pyenv`, a tool used to manage multiple versions of python in your machine
	* Windows [installation guide](https://github.com/pyenv-win/pyenv-win/blob/master/docs/installation.md#powershell) for `Powershell` or `GitBash` 
		* pip users try either: 
			* `pip install pyenv-win --target %USERPROFILE%\\.pyenv.  
			* `pip install pyenv-win --target %USERPROFILE%\\.pyenv --no-user --upgrade`
	* Linux [installation guide](https://github.com/pyenv/pyenv?tab=readme-ov-file#linuxunix) 
		* `curl -fsSL https://pyenv.run | bash`
	* MacOS: `brew install pyenv`
3. Using `pyenv` install *on your system* the python version you want
	* Example: `pyenv install 3.8.18` or `pyenv install 3.10`
		* Check the installation with:
			* `pyenv which python3.8` or `pyenv which python3.10`
			It should return *the path* where that specific python version (specifically the interpreter!) is installed
			* `/Users/your-username/.pyenv/versions/3.8.18/bin/python3.8`
4. Now navigate to the folder where your project will live
	   `cd ~/path_to_project`
5. In the project folder, create a virtual environment *using the specific python version interpreter* that you need for your project using a specified name to the environment
		`virtualenv -p ~/.pyenv/versions/3.8.18/bin/python3.8 .my_new_env`
		or `virtualenv -p $(pyenv which python3.8) .my_new_env`
6. Activate/Source the virtual environment
	* Linux/MacOS: `source .my_new_env/bin/activate`
	* Windows: `.my_new_env/Scripts/activate`
7. Verify the python version in the environment with the commands in step 1.

## Tip 
You can *deactivate* the virtual environment at any time with: `deactivate`

## You're all set!
Your project is now running in an isolated environment with the exact Python version you specified. 
