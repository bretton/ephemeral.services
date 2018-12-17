# Bitcoind node

The following is adapted from the guide at (https://gist.github.com/itoonx/95aec9a3b4da01fd1fd724dffc056963)

## Install from source

### Ensure your system is up to date
```
sudo apt-get update
sudo apt-get upgrade
````

### Install dependancies that may be required
Install dependencies there might be more based on your system. The following is for a fresh Ubuntu install/server.
```
sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev
sudo apt-get install libboost-all-dev
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
sudo apt-get install libqrencode-dev autoconf openssl libssl-dev libevent-dev
sudo apt-get install libminiupnpc-dev
```

### Download the source code
Download the Bitcoin source code as follows
```
cd ~
git clone https://github.com/bitcoin/bitcoin.git
```

Bitcoin uses the Berkley DB 4.8, we need to install it as well. Download & Install Berkley DB:
```
cd ~
mkdir bitcoin/db4/
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
tar -xzvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix/
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=/home/YOURUSER/bitcoin/db4/
make install
```

### Verifying it is valid
To-do: add steps to verify the sources are valid

### Compiling from source
Compile Bitcoin with Berkley DB 4.8:
```
cd ~/bitcoin/
./autogen.sh
# below command ./configure may return with error for dependencies
# you need to make sure that it returns with no error
# If it does please install the dependencies and rerun the /autogen.sh command again and then below command again
./configure LDFLAGS="-L/home/YOURUSER/bitcoin/db4/lib/" CPPFLAGS="-I/home/YOURUSER/bitcoin/db4/include/"

# below command may take 5-10 minutes based on your system
make -s -j5

# If all went well you will be able to access the binary at below location
cd ~/bitcoin/
./src/bitcoind
./src/bitcoin-qt
./src/bitcoin-cli
```

### Installing for your platform
To-do: list the steps of copying the binaries to appropriate locations

# Bitcoind Ubuntu PPA

The following is adapted from the guide at (https://gist.github.com/bretton/0b22a0503a9eba09df86a23f3d625c13)

### Installation

First add the repository:
```
sudo apt-add-repository ppa:bitcoin/bitcoin
```

You will be prompted for your user password. Provide it to continue, and press enter when prompted.

Run the update process:
```
sudo apt-get update
```

Then proceed with installing `bitcoind` as follows:
```
sudo apt-get install bitcoind
```

Setup your .bitcoin/bitcoin.conf file, there is a sample here:  
> https://github.com/bitcoin/bitcoin/blob/master/contrib/debian/examples/bitcoin.conf

Simplest version might be as follows:
```
server=1
txindex=1
daemon=1
externalip=X.X.X.X
maxconnections=10
rpcuser=REPLACEME
rpcpassword=REPLACEME
minrelaytxfee=0.00000000
incrementalrelayfee=0.00000010
zmqpubrawblock=tcp://127.0.0.1:28332
zmqpubrawtx=tcp://127.0.0.1:28333
```

Start `bitcoind` to initiate sync, and be sure to take a look at https://en.bitcoin.it/wiki/Running_Bitcoin
```
bitcoind
```

You can monitor the progress in the logs:
```
tail $HOME/.bitcoin/debug.log
```

**Important:** The `bitcoind` sync process can take 3 to 5 days to complete! 

### (Optional) You can monitor sync progress with the following:

Install `jq` to trim the json:
```
sudo apt-get install jq
```

Get the current block count and run it through `jq`:
```
curl -s https://api.smartbit.com.au/v1/blockchain/blocks |jq -r -c .blocks[0].height
```

Compare the result to the output of:
```
bitcoin-cli getblockcount
```

Alternatively, combine both of these into a convenient one-liner expression:
```
echo `bitcoin-cli getblockcount 2>&1`/`curl -s https://api.smartbit.com.au/v1/blockchain/blocks |jq -r -c .blocks[0].height 2>/dev/null`
```

If the output values are the same, then your `bitcoind` node is fully synced.

### Setting up bitcoind to start automatically

To setup `bitcoind` to start automatically, we'll borrow the systemd setup from the following guide:

> Adapted from sources
> * https://medium.com/@stadicus/perfect-low-cost-%EF%B8%8Flightning%EF%B8%8F-node-4c2f42a4ff7b  
> * https://gist.github.com/Stadicus/0f6df973c936d74200298dee3d50b688#file-bitcoind-service  

First make sure `bitcoind` is fully synced, then stop it with:
```
bitcoin-cli stop
```

Add & edit the systemd configuration file:
```
sudo nano /etc/systemd/system/bitcoind.service
```

Add in the following text, making sure to replace all instances of USERNAME with your username, or the applicable username `bitcoind` is being run as:
```
[Unit]
Description=Bitcoin daemon
After=network.target

[Service]
User=USERNAME
Group=USERNAME
Type=forking
PIDFile=/home/USERNAME/.bitcoin/bitcoind.pid
ExecStart=/usr/bin/bitcoind -conf=/home/USERNAME/.bitcoin/bitcoin.conf -pid=/home/USERNAME/.bitcoin/bitcoind.pid
KillMode=process
Restart=always
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target
```

Enable the systemd setup with the following commands:
```
sudo systemctl enable bitcoind
sudo systemctl start bitcoind
```

Check it's working properly with:
```
systemctl status bitcoind
```

`bitcoind` should start automatically from here on, and restart if it fails. 

# BTCD node

## Install from source

### Downloading the source code

### Verifying it is valid

### Compiling from source

### Installing for your platform