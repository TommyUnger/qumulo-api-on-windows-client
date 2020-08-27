# How to get up and running with the Qumulo API and qq tool from a Windows client machine

Running the Qumulo API and qq tool from Windows client machine is a pretty quick process thanks to the easy of installation of python and the Qumulo API tools. This readme will guide you through the steps to get up and running.

## Requirements

* **A Windows 10 machine or VM**. In theory, other Windows platforms will work, but this is the one I've tested.
* **Qumulo storage API access**

### Installing Python software and virtual environment on Windows 10

1. Download the Windows x86-64 executable installer from: https://www.python.org/downloads/windows/ (in the example, this is what we installed: https://www.python.org/ftp/python/3.7.9/python-3.7.9-amd64.exe)
1. Run the install and during the install, check the box at the bottom, *Add to path*.
1. Open a command prompt: cmd.exe
1. Run the command `C:\> where python.exe` to find where the python.exe is installed
1. Create a virtual environment with the newly installed python
    ```C:\> C:\Users\qumulo\AppData\Local\Programs\Python\Python37\python.exe -m venv python369-qumulo```


### Run python virtual environment, install python bindings and libraries

1. From the command prompt, activate the virtual environment
    ```C:\> python379-qumulo\Scripts\activate.bat```
1. Upgrade pip
    ```C:\> python.exe -m pip install --upgrade pip```
1. Install the python bindings and tools
    ```C:\> pip install qumulo_api jupyter```

At this point you're fully up and running with the latest Qumulo API bindings and tools as well as the Jupyter python notebook tool which is a great way to explore the Qumulo API python bindings.


### Use the Qumulo API via the qq command line interface tool (CLI)

Let's first see if we can connect to our Qumulo cluster with the included `qq.exe` tool that's part of the `qumulo_api` package.

1. From the command prompt run the following command. First change your host and user to your own cluster and credentials though:
    ```C:\> qq.exe --credentials-store qumulo-creds.txt --host product.eng.qumulo.com login -u AD\tommy```
1. See who you're logged into the Qumulo as:
    ```C:\> qq.exe --host product.eng.qumulo.com who_am_i```
1. See the filesystem aggregate data at the top of the filesystem:
    ```C:\> qq.exe --host product.eng.qumulo.com fs_read_dir_aggregates --path / --max-entries 0```

### Use the Qumulo API via Jupyter notebook

1. From the command prompt run the following command
    ```C:\> jupyter-notebook```
1. Create a new python3 notebook by clicking the New button in the upper right
1. Paste in the following code 
    ```
    import pprint
    from qumulo.lib.auth import get_credentials
    from qumulo.rest_client import RestClient

    rc = RestClient('product.eng.qumulo.com', 8000, get_credentials('qumulo-creds.txt'))

    print("\n#### who am i ####")
    pprint.pprint(rc.auth.who_am_i())

    print("\n#### read dir ####")
    pprint.pprint(rc.fs.read_dir_aggregates(path='/', max_entries=0))
    ```

