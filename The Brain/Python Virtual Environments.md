2025-02-03 14:05

Status: #Bud 

Tags: #Python #PythonModule #PythonLibrary 

# Python Virtual Environments
Because Python relies on external libraries installed through the "pip" tool, in different projects we will be using different libraries and versions. Because of this, a single global installation of Python is insufficient, as we will need access to different libraries and versions in each of our projects, causing compatibility issues. **Virtual Environments** solve this by allowing us to have a separate set of libraries for each individual project. 

There are many ways to create a virtual environment, but the `venv` tool is native to Python and is the most commonly used:
- Navigate to the directory of your python project
- Create environment: `python3 -m venv env` 
	- This will create an `env` dir to contain the virtual environment
- Activate Environment (cmd prompt):  `env\scripts\activate.bat`
	- Will prefix path with (env) if successful
	- To activate using GitBash, `cd` into `scripts` dir, then `. activate`
		- If successful, will see a (env) tag after a command is given
- Deactivate: `deactivate

Once you have created and activated an environment, any packages or libraries that we install will be localized to this environment only.
- View installed programs with `pip list` command

By nature, environments are not exportable. We cannot simply download the `env` dir and expect the code to work properly. Instead, we can create a `requirements.txt` file that we use to recreate the virtual environment on other machines
- Create: `pip freeze > requirements.txt`
- Use: `pip install -r requirements.txt`
## References
