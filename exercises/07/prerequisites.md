# Prerequisites

There are hardware and software prerequisites for completing this exercise.

## Hardware

Each participant must have their own laptop with enough admin privileges to be able to install software. The laptop must also be able to connect to the Internet.

## Software

Before the virtual event, participants must ensure they have the following installed on their laptops:

- Python (3+ version) : [https://www.python.org/downloads](https://www.python.org/downloads)
- Create a Python virtual environment
- An approved SAP Ariba developer application

Below, steps to install Python in Windows:

1. Download Python from the Python website: https://www.python.org/downloads/release/python-382/
1. Install virtualenvwrapper: https://pypi.org/project/virtualenvwrapper-win/
	```bash
	pip install virtualenvwrapper-win
	```
1. Define environment variable in Windows: Add an environment variable `WORKON_HOME` to specify the path to store environments. By default, this is `%USERPROFILE%\Envs`.
1. Navigate to directory `scripts`, create virtual environment and install packages required to run `ariba_api_authentication.py`
	```bash
    cd scripts
	mkvirtualenv ariba-api-authentication -r requirements.txt
	pip install -r requirements.txt
	```
1. Ensure to activate the Python environment:
    ```bash
    workon ariba-api-authentication
    ```
