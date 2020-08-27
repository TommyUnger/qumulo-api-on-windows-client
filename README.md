# Using the Qumulo API and qq tool on Windows

Running the Qumulo API and qq (command line interface) tool from a Windows client machine is a pretty quick process thanks to the ease of installation for python and the Qumulo API tools. This readme will guide you through the steps to get up and running in minutes.


## Requirements

* **A Windows 10 machine or VM**. In theory, other Windows platforms will work, but this is the one I've tested.
* **Qumulo storage API access**

---


### Install Python software and virtual environment on Windows 10

1. Download the Windows x86-64 executable installer from: https://www.python.org/downloads/windows/ (in the example, this is what we installed: https://www.python.org/ftp/python/3.7.9/python-3.7.9-amd64.exe)
1. Run the install and during the install, check the box at the bottom, *Add to path*.
1. Open a command prompt via `cmd.exe`
1. Create a directory for these scripts and cd to that directory:
    ```
    C:\> md \qumulo-work
    C:\> cd \qumulo-work
    ```
1. Find our where python is installed with the following command:
    ```C:\qumulo-work\> where python.exe```
1. Create a virtual environment with the newly installed python:
    ```C:\qumulo-work\> C:\Users\qumulo\AppData\Local\Programs\Python\Python37\python.exe -m venv python379```

---


### Run a python virtual environment and install python libraries

1. From the command prompt, activate the virtual environment
    ```C:\qumulo-work\> python379\Scripts\activate.bat```
1. Upgrade pip
    ```C:\qumulo-work\> python.exe -m pip install --upgrade pip```
1. Install the python bindings and tools
    ```C:\qumulo-work\> pip install qumulo_api jupyter```

At this point you're fully up and running with the latest Qumulo API bindings and tools as well as the Jupyter python notebook tool which is a great way to explore and use the Qumulo API python bindings.

---


### Use the Qumulo API via the qq.exe command line interface(CLI) tool

First, we connect to a Qumulo cluster with the included `qq.exe` CLI tool that is part of the `qumulo_api` package. Then we'll run a few simple commands.

1. From the command prompt run the following command. First change your host and user to your own cluster and credentials though:
    ```C:\qumulo-work\> qq.exe --credentials-store qumulo-creds.txt --host qumulo.demo login -u AD\tommy```
1. See who you're logged into the Qumulo as:
    ```C:\qumulo-work\> qq.exe --host qumulo.demo who_am_i```
1. See the filesystem aggregate data at the top of the filesystem:
    ```C:\qumulo-work\> qq.exe --host qumulo.demo fs_read_dir_aggregates --path / --max-entries 0```

---


### Use the Qumulo API via Jupyter notebook

1. From the command prompt run the following command
    ```C:\qumulo-work\> jupyter-notebook```
1. Create a new python3 notebook by clicking the New button in the upper right
1. Paste in the following code 
    ```
    import pprint
    from qumulo.lib.auth import get_credentials
    from qumulo.rest_client import RestClient

    rc = RestClient('qumulo.demo', 8000, get_credentials('qumulo-creds.txt'))

    print("\n#### who am i ####")
    pprint.pprint(rc.auth.who_am_i())

    print("\n#### read dir ####")
    pprint.pprint(rc.fs.read_dir_aggregates(path='/', max_entries=0))
    ```

