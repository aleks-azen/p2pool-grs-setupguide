# Groestlcoin (GRS) P2Pool setup guide

## Installing Groestlcoin-core
The first thing you will need is the groestlcoin-core wallet: https://www.groestlcoin.org/groestlcoin-core-wallet/ download this, run it and let it download the blockchain

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

### Clone P2Pool

Open a terminal window and navigate to where you want to download P2Pool. Type:

    git clone https://github.com/Groestlcoin/p2pool-grs.git
	cd p2pool-grs
	
Leave the terminal open and in that directory (p2pool-grs/) create a file called requirements.txt and paste this into it:

	Twisted>=12.2.0
	argparse>=1.2.1
	pyOpenSSL>=0.13

Next type:

    pip install -r requirements.txt

### Compile the Groestlcoin hash module

You will need to compile the groestlhash module. On Windows you will need to install MinGW.

MinGW for windows (Run these installs as administrator if possible):
You can use the official version http://www.mingw.org/wiki/Getting_Started
`I'd recommend the unoffical one for python however` - https://github.com/develersrl/gccwinbinaries (direct download - https://github.com/develersrl/gccwinbinaries/releases/download/v1.1/gcc-mingw-4.3.3-setup.exe)

If using the unoficial python one make sure to check all of the boxes during the install and select `link with MSVCR90.DLL`


`sudo python setup.py install` On Linux  
`python.exe setup.py build --compile=mingw32 install` On Windows  
`cd ..`

### Run P2Pool

Type `python run_p2pool.py --net groestlcoin`.

---

*Donations for this guide: FbrrBNMpGugG6VRkKAjqQ2JAo3LcB9F77k