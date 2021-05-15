
# Make a Python Script as Linux Debian Installer

Write a short code

* Create a folder myapp


* Create a file in myapp directory named main.py (You could name it anything other than main.py).

* Open main.py and paste the following code.

```python
import tkinter as Tkinter


def main():
	top = Tkinter.Tk()
	top.geometry("500x500")
	B = Tkinter.Button(top, text ="Ghanshyam Maharaj")
	B.pack()
	top.mainloop()

if __name__ == '__main__':
	main()
```

## Create a virtual environment

* Open terminal in myapp directory and type

```bash
python3 -m venv env
```
**NOTE:** python3 is necessary, it will not work on python2. So take care of this thing.


## Test the Virtual Environment
```bash
source env/bin/activate  
```
If you see `(env)`, it indicataes virtual environment is activated succesfully.

## Test the code

python3 main.py

This will display a small GUI application that says, **Ghanshyam Maharaj**

(It does not matter if you write `python3 main.py` or `python main.py`. It will use the python version which you used while creating a virtual environment)


## Install cx_Freeze

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install cx_Freeze.
```
pip install cx_Freeze
```
Check [here](https://cx-freeze.readthedocs.io/en/latest/#welcome-to-cx-freeze-s-documentation) if cx_freeze is compatible with the python version you are using else it will throw some errors.

## Create setup.py file in the directory.

You could name it anything. Copy the following code.

```python3
from cx_Freeze import setup, Executable


# Dependencies are automatically detected, but it might need fine tuning.
# build_exe_options = {"packages": ["os"], "excludes": ["tkinter"]}
build_exe_options = {"packages": ["os"], "excludes": []}

setup(
    name="Ghanshyam",
    url='<url to your github code>',  # Not necessary
    version="1.0.0",  # [Learn how to specify verion no. at https://semver.org/
    description="My GUI application which displays Ghanshyam Maharaj",
    options={
        # https://cx-freeze.readthedocs.io/en/latest/distutils.html#build-exe
        "build_exe": build_exe_options,
    },
    executables=[
        Executable("main.py",
                   icon=r"<path\to\icon\file\icon.ico>",  # Must be .ico only,
                   )
    ]
)
```
