===========
Get Started
===========

======================================
PYTHON AND MOBLABPY INSTALLATION GUIDE
======================================

Installing and managing packages in Python is complicated, there are a number of alternative solutions for 
most tasks. This guide tries to give the reader a sense of the best (or most popular) solutions, and give 
clear recommendations. It focuses on users of Python and the Moblabpy on common operating systems and 
hardware.

PYTHON AND MOBLABPY INSTALLATION (WINDOWS USERS)
------------------------------------------------

1. Check if Python is already installed (You can type `python --version` on cmd). If you have already installed Python, you can skip the next two steps, otherwise, please follow the next step to install Python.
2. Install Python `here <https://www.python.org/downloads/>`_ and select the most recent version. 
3. Follow the instruction of the installer and finish installation. Make sure to tick the option `Add Python 3.x (the version that you download) to PATH` at the start of the installer window.
4. After the installation finished, press ``Win`` key, type cmd and hit ``Enter`` .
5. Type ``pip install moblabpy`` in the cmd to install Moblabpy.

PIP IS NOT RECONGNIZED AS AN INTERNAL OR EXTERNAL COMMAND
---------------------------------------------------------

If you see the ``pip is not recognized as an internal or external command`` when you type ``pip install moblabpy``
in the cmd. It means that you forgot to add Python in PATH during the installation of Python. You can add it 
by the following step:

1. Press ``Win + R`` , type ``%AppData%`` and hit ``Enter``
2. Back out one directory from Roaming
3. Navigate to ``Local/Programs/Python/Python39/Scripts``
4. Copy the file path by selecting the whole directory in the address bar
5. Press ``Win``, type Control Panel and hit ``Enter``
6. Navigate to System and Security → System → Advanced System Setting → Environment Variables
7. Select Path in the top box and Edit
8. Select New and paste the file path copied from step 4

PYTHON AND MOBLABPY INSTALLATION (MAC USERS)
--------------------------------------------

1. Same as step 1 to 3 of window users (terminal for mac users)
2. After the installation finished, press ``Cmd`` and the space bar simultaneously, type ``terminal`` and hit ``Enter`` .
3. Type ``pip install moblabpy`` in the cmd to install Moblabpy.

PIP: COMMAND NOT FOUND
---------------------------------------------------------

If you see the ``pip: command not found`` when you type ``pip install moblabpy`` in the terminal. 
It means that you forgot to add Python in PATH during the installation of Python. You can add it 
by the following step:

1. Type ``which python3`` in the terminal and the path of where python is located will be shown
2. Type ``echo $PATH`` in the terminal and it will show the path of your machine to look for command
3. Navigate back to home file by typing ``cd``
4. Type ``nano .bash_profile`` in the terminal and hit ``Enter``
5. Type ``PATH="path/of/bin/folder/that/you/install/your/python:${PATH}"`` and ``export PATH`` in the next line and save the changes
6. restart the terminal and it should be fine now

MORE DETAILS
------------

For more details, you can go to `MAC <https://docs.python.org/3/using/mac.html>`_ if you are mac users or
`WINDOWS <https://docs.python.org/3/using/windows.html>`_ if you are windows user. 
