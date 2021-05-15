
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
        Executable("main.py")
    ]
)
```

## Create a Portable Application

## Build the code
```bash
python setup.py build
```


### Cannot find patchelf ?
It might give an error such as

> **ValueError: Cannot find required utility `patchelf` in PATH**

```bash
sudo apt install patchelf
```
and again run `python setup.py build`

### If everything goes well

It will take a while and after the process is completed you will see a folder named build in your directory where you will have a folder named **exe.linux-\<architecture>-\<python-verion>**

Open that folder and you will find **main**

Now, you can share this build folder with anyone and they can use this script when they will execute **main**

To execute main open terminal and type `./main`

## Create an Executable Debian Installer

You can turn off *Virtual Environment* if you wish.

Now to build a folder and you will have a file that will look something like this.

>exe.linux-x86_64-3.8

**NOTE: We will be using this name(exe.linux-x86_64-3.8) throughout this page. It might differ in your computer.**

### Go to exe.linux-x86_64-3.8

```bash
cd exe.linux-x86_64-3.8
```

### Create a directory and named **usr** and *move* all files to **usr* folder.

```bash
mkdir usr
mv * usr/
```

### Create a directory and named **DEBIAN**(all in capital) and go inside it.
```bash
mkdir DEBIAN
cd DEBIAN/
```

## Create Control File
**Create a new file named `control`**
```bash
vi control
```

Paste the following code
```control
Package: Ghanshyam
Version: 1.0.0
Architecture: all
Essential: no
Priority: optional
Depends: less, parted, rsync
Maintainer: Ghanshyam Maharaj
Architecture: amd64
Description: Harikrushna Maharaj, Jai Swaminarayan.
```

## Create preinst File

> Preinst is a bash script that will run before installation. It is usually used to check if any configuration files need to be removed or not. And path to older icons are removed. If old icons are kept, new icons are sometimes not replaced. You can view in [this video](https://youtu.be/ep88vVfzDAo?t=1092) and skip to 18:12
 
## Create a new file named `preinst`
```bash
vi preinst
```

### And paste the follwing code.
```bash
#!/bin/bash
echo "Jai Swaminarayan, preinst script"
```
### Change rights to preinst script

```bash
chmod 755 preinst
```

## Go to `usr` directory
```bash
cd ../usr/
```

## Create `bin` folder and move all files inside it
```bash
mkdir bin/
mv * bin/
```


## Create `share` folder and go inside it
```
mkdir share/
cd share/
```

## Create *icons* directory and place the application icon
```bash
mkdir icons
cd icons
```
(Paste the icon file in this directory)

> PNG format is also supported. .ico file is Not mandatory.

## Create a `.desktop` file inside `applications` directory
```
cd ..
mkdir applications
cd applications
vi ghanshyam.desktop
```

Paste the following code
```
[Desktop Entry]
Version=1.0
Name=ghanshyam
Comment=Comment in ghanshyam.desktop file
Exec=/usr/bin/main
Icon=/usr/share/icons/icon.png
Terminal=false
Type=Application
Categories=Utility;Application;

```
> Replace `Exec=/usr/bin/main` with your **main application file**

> Replace `Icon=/usr/share/icons/icon.png` with your **icon directory path**

> Turn `Terminal=true` if you want to display terminal while your application is running.

**NOTE: *"exe.linux-x86_64-3.8"* will be considered as root directory while creating Debian installer. Thus \<\/\> is included before `usr` directory**


### Change file permission
> One last thing to add is that by setting executable rights to your .desktop file, it automatically takes the specified Icon and Name (specified in the corresponding fields), as it should be. Be careful though, the filename doesn't really change, it still remains 'launcher_name_here.desktop' and not 'Name_field_here', the system chooses to display it like 'Name_field_here' because it's nicer without the .desktop extension. 

Source: [Ubuntu Docs](https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles)

```bash
chmod +x ghanshyam.desktop
```

### Goto `build` directory and create .deb file
```
dpkg-deb --build exe.linux-x86_64-3.8/
```

> dpkg-deb: building package 'ghanshyam' in 'exe.linux-x86_64-3.8.deb'.

### Now you can see that new `.deb` file is created

```bash
ls
```
> exe.linux-x86_64-3.8  exe.linux-x86_64-3.8.deb


## Install the .deb file
```bash
sudo dpkg -i exe.linux-x86_64-3.8.deb
```

Now you can search for **ghanshyam** in launcher and your new application is also added to favourites.


## Error in creating installer

**If the installer throws Architecture Error. You might need to replace \<Architecture: all> with \<Architecture: amd64> as per system.**



## References
[Ubuntu Docs](https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles)

[cx_freeze | Docs](https://cx-freeze.readthedocs.io/en/latest/)

[cx_freeze | GitHub](https://github.com/marcelotduarte/cx_Freeze)

[cx_freeze | distutils setup script](https://cx-freeze.readthedocs.io/en/latest/distutils.html)

[Youtube | How to Create An .exe And Installer For Python Application using cx_Freeze Module in HINDI | Urdu](https://youtu.be/inuzQfxTOkg)
