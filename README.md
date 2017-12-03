# Groestlcoin (GRS) P2Pool setup guide
One of my installs refused to function on a 64-bit version of python 2.7, installing the 32-bit version and reinstalling all of the dependencies fixed that -- if you are having issues try that

## Installing Groestlcoin-core
The first thing you will need is the groestlcoin-core wallet: 

Windows: https://www.groestlcoin.org/groestlcoin-core-wallet/ download this, run it and let it download the blockchain

Linux: 

	sudo apt-add-repository ppa:groestlcoin/groestlcoin
	sudo apt-get update
	sudo apt-get install groestlcoind
	groestlcoind -daemon

`Linux notes: the above will download and start the groestlcoin core daemon which starts to download the blockchain you can continue with the rest of the guide while this downloads`

To check the blockchain download progress you can type `groestlcoin-cli getblockchaininfo` you can start running the p2pool when  "verificationprogress" value is .99xxxxx

### Windows users with multiple harddrives
You may not want to store groestlcoins entire blockchain on your primary drive. To move the blockchain to a different drive follow these steps (I will be moving to E:\groestldata):

1. `Create your target directory in my case it would be E:\groestldata. Move everything from C:\Users\your_user\AppData\Roaming\groestlcoin (%appdata%\groestlcoin) to your newly created directory. If you already have the blockchain downloaded on your primary drive you can attempt to shutdown the core wallet and copy it over to, if something goes wrong this may corrupt it and force you to redownload the chain.`
2. `Delete your C:\Users\your_user\AppData\Roaming\groestlcoin directory (YOUR WALLET IS HERE BACK IT UP/MAKE SURE ITS MOVED)`
3. open a command promt and type:

		cd %appdata%
		mklink /D groestlcoin E:\groestldata
	
4. `What we've just done is create a symbolic link on your system so when Groestlcoin-core tries to write data in the standard directory it will be redirected to your other drive`

## Configuring the Groestlcoin daemon

After you installed Groestlcoin-core you will need to enable the RPC server. To do so add the following text to your `groestlcoin.conf` file:

    server=1
    rpcuser=user
    rpcpassword=YourSuperGreatPasswordNumber_DO_NOT_USE_THIS_OR_YOU_WILL_GET_ROBBED_385593

The `groestlcoin.conf` file can be found in the data directory (datadir):

Operating System | Default datadir | Typical path to configuration file
--- | --- | ---
Windows | `%APPDATA%\Groestlcoin\` | `C:\Users\username\AppData\Roaming\Groestlcoin\Groestlcoin.conf`
Linux | `$HOME/.Groestlcoin/` | `/home/username/.Groestlcoin/Groestlcoin.conf`
Mac OSX | `$HOME/Library/Application Support/Groestlcoin/` | `/Users/username/Library/Application Support/Groestlcoin/Groestlcoin.conf`

If it is not there simply create it yourself. Restart the Groestlcoin daemon after you edited `Groestlcoin.conf`.

## Running from source

### Requirements

Terminal commands are provided for Ubuntu/Debian. Start with `sudo su`.

Requirement | Terminal command | Notes
--- | --- | ---
[Git](https://git-scm.com/downloads) | `apt-get install git` |
[Python 2 (NOT 3)](https://www.python.org/downloads/) | `apt-get install python python-pip` | On Windows check the options to install PIP and to add Python to the PATH variable.
[win32api](http://sourceforge.net/projects/pywin32/files/pywin32/Build%20218/) |  | install the same bit version as your python install -- windows only
[win32api wmi wrapper](https://pypi.python.org/pypi/WMI/#downloads) |  | -- windows only


### Clone P2Pool

Open a terminal window and navigate to where you want to download P2Pool. Type:

    git clone https://github.com/Groestlcoin/p2pool-grs.git
	cd p2pool-grs
    pip install -r requirements.txt

### Compile the Groestlcoin hash module (OPTIONAL)

####  This tends to be the most annoying part so I've included a precomipled module for windows with this project. -- I've merged the module into the core repo requirements.txt so this is optional now

You will need to compile the groestlhash module. On Windows you will need to install MinGW.

MinGW for windows (Run these installs as administrator if possible):

You can use the official version http://www.mingw.org/wiki/Getting_Started

`I'd recommend the unoffical one for python however` - https://github.com/develersrl/gccwinbinaries (direct download - https://github.com/develersrl/gccwinbinaries/releases/download/v1.1/gcc-mingw-4.3.3-setup.exe)

If using the unoficial python one make sure to check all of the boxes during the install and select `link with MSVCR90.DLL`

`Once all of the appropriate software is setup type:` 
	
	git clone https://github.com/GroestlCoin/groestlcoin-hash-python 
	cd groestlcoin_hash_python

followed by

`sudo python setup.py install` On Linux  
`python.exe setup.py build --compile=mingw32 install` On Windows  
`cd ..`

## Finally Run P2Pool

Type `python run_p2pool.py --net groestlcoin`.

This runs p2pool at localhost:11330 so to mine point your miner at that address

You can open it to the public by forwarding port 11330 from your router to the computer running the p2pool on your network and enabling uPNP


Lastly here is my favorite web-ui to use for p2pool: https://github.com/justino/p2pool-ui-punchy simply clone that and put its contents into p2pool-grs/webstatic

---

*Donations for this guide: FbrrBNMpGugG6VRkKAjqQ2JAo3LcB9F77k