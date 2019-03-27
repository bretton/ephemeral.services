# Lightning Node

## LND
The following has been adapted from the guide at (https://gist.github.com/bretton/0b22a0503a9eba09df86a23f3d625c13)

### Install Go
The `lnd` install guide refers to golang-1.10-go, but Ubuntu 16.04 LTS currently has golang-1.9-go. To install the latest `go` using `snap` instead:
```
sudo snap install --classic go
```

On success you will see a result like:
```
go 1.11.X from Michael Hudson-Doyle (mwhudson) installed
```

Create a 'go' directory in your home directory:
```
mkdir go
```

Set `go` paths, this can be done in `.profile` (which reads `.bashrc`) or directly in `.bashrc`: 

> You might notice some difference between ssh sessions and local terminal sessions with .profile so for the purpose of this guide we'll use `.bashrc` as the results are the same for both types of session

```
nano .bashrc
```

Add to the end:
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

Important: the `snap` install of `go` will automatically set the GOROOT variable. It's no longer necessary to set this yourself, unless using `apt-get install` for older version of `go`.

Logout and log back in to reread variables, or you can type:
```
source .bashrc
```

Check variables with:
```
go env
```

And if you see output, `go` is setup correctly.

### Install build tools

Make sure you have the build-essential package installed:
```
apt-get install build-essential
```

### Install LND

Install `lnd` by cloning the source, running `dep`, and then the build commands as follows:
```
go get -d github.com/lightningnetwork/lnd
cd $GOPATH/src/github.com/lightningnetwork/lnd
make && make install
```

**Important:** this next step requires `bitcoind` be fully synced!

Run `lnd` for first run as follows, it will take a while to catch up and perform its own sync:
```
lnd --bitcoin.active --bitcoin.mainnet --debuglevel=debug --bitcoin.node=bitcoind --bitcoind.rpcuser=REPLACEME --bitcoind.rpcpass=REPLACEME --externalip=X.X.X.X --bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332 --bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333
```

But before the sync actually starts, in another terminal window you will need to create a wallet, store the recovery key, and then issue the unlock command.

To create a wallet run:
```
lncli create
```

An example of the output is as follows. Please ensure you have a strong password with minimum 8 characters, and mix of uppercase and lowercase: 

```
Input wallet password: ********
Confirm wallet password: ********

Do you have an existing cipher seed mnemonic you want to use? (Enter y/n): n

Your cipher seed can optionally be encrypted.
Input your passphrase you wish to encrypt it (or press enter to proceed without a cipher seed passphrase): ********
Confirm cipher seed passphrase: ********

Generating fresh cipher seed...

!!!YOU MUST WRITE DOWN THIS SEED TO BE ABLE TO RESTORE THE WALLET!!!

------------------BEGIN LND CIPHER SEED------------------
 1. one        2. two        3. three        4. four 
 5. five       6. six        7. seven        8. eight
 9. nine      10. ten       11. eleven      12. twelve 
13. thirteen  14. fourteen  15. fifteen     16. sixteen
17. seventeen 18. eighteen  19. nineteen    20. twenty
21. twentyone 22. twentytwo 23. twentythree 24. twentfour
------------------END LND CIPHER SEED--------------------
```

Copy and paste the cipher seed and store it securely somehow, such a secure note in a password manager, or printed to paper and locked in a safe, or similar.

You will now have to unlock the wallet you just created so `lnd` can proceed with the initial sync:
```
lncli unlock
```

Once sync is complete, you can `<ctrl-c>` the lnd process, and proceed with editing a config file so you can start `lnd` with fewer flags on next launch.

Create and edit $HOME/.lnd/lnd.conf from  
> https://github.com/lightningnetwork/lnd/blob/master/sample-lnd.conf

A very simple version might be as follows, you can add more from the info in the sample config later:
```
[Application Options]
unsafe-disconnect=1
; set external IP if not using NAT
externalip=YOURIP
; set node alias (seen in explorers)
alias=SET-YOUR-ALIAS
; the workers require a recent master from end March 2019
; https://github.com/lightningnetwork/lnd/pull/2736
workers.read=100
workers.write=100
workers.sig=100

[Bitcoin]
bitcoin.active=1
bitcoin.mainnet=1
bitcoin.node=bitcoind

[Bitcoind]
bitcoind.rpcuser=REPLACE
bitcoind.rpcpass=REPLACE
bitcoind.zmqpubrawblock=tcp://127.0.0.1:28332
bitcoind.zmqpubrawtx=tcp://127.0.0.1:28333
```

Thereafter you can start `lnd` without flags, or using `supervisor` (see below) or some other option like systemd:
```
lnd
```

You monitor the log file in the logs directory under $HOME/.lnd/logs/bitcoin/mainnet/lnd.log

To verify `lnd` is operational, open another terminal window or session and try the following:
```
lncli getinfo
lncli getnetworkinfo
lncli describegraph
```

Setup a wallet to deposit some bitcoin:
```
lncli newaddress p2wkh
```
This will produce a Bech32 format address, which is native segwit address format, with lower fees in fee calculations for opening/closing channels. 

> If you have troubles depositing to bech32 address from places like earn.com or exchanges which don't yet support segwit addressing, you can also use `lncli newaddress np2wkh` to get a different format address that looks more like a traditional multisig address, but won't be able to take advantage of segwit fees benefits.

Make the deposit and check progress with:
```
lncli walletbalance 
```

A confirmed a balance means the deposit is complete and ready for opening channels.

To open your first channel(s) it may be useful to find active nodes. You can do this by browsing the node directory at various explorers such as https://www.robtex.com/lightning/node/ or https://1ml.com/node

Open a channel by first connecting, then opening a channel as follows:
```
lncli connect pubkey@ip:port
lncli openchannel --node_key=<pubkey> --local_amt=<amount>
```

Helpful hint:  
> if mSATs, satoshis, bits, mBTC are confusing, consider installing this useful calculator
> https://github.com/jb55/bcalc

Verify pending & open channels using:
```
lncli pendingchannels
lncli listchannels
```

### Optional: Setup Supervisor to run LND automatically

`supervisor` is one way to start `lnd`  automatically, and automatically restart if it fails.

First exit any running `lnd` session, and proceed through the following steps:

First install `supervisor`:
```
sudo apt-get install supervisor
```

Then setup the configuration file, `sudo` access will be required:
```
cd /etc/supervisor/conf.d
sudo nano lnd.conf
```

Edit `lnd.conf` and add the following:
```
[program:lnd]
user=REPLACE-WITH-YOUR-USERNAME
command=/home/USERNAME/go/bin/lnd --configfile=/home/USERNAME/.lnd/lnd.conf
startretries=999999999999999999999999999
autostart=true
autorestart=true
```

Reload `supervisor`:
```
sudo supervisorctl reload
```

You can monitor the log files by tailing them in the relevant directories in your home directory for `lnd`, or in /var/log/supervisor/ with `sudo` access.

**Important:** If `lnd` restarts via supervisor for some reason, you will need to unlock your wallet again for the node to go live.
```
lncli unlock
```

#### Email notification of supervisor restart
If you wish to be notified by email when supervisor restarts a process, you can install the `superlance` plugin:
```
sudo apt-get install python-pip
sudo -H pip install superlance
```

Edit supervisor's config file:
```
sudo nano /etc/supervisor/supervisord.conf
```

Add the following, changing the email address to your email address:
```
[eventlistener:crashmail]
command=/usr/local/bin/crashmail -p lnd -m your@email.address
events=PROCESS_STATE
```

This should be followed by updating supervisor, which will also restart `lnd`, and add the event notifier you've configured: 
```
sudo supervisorctl update
```

So be sure to unlock your wallet again:
```
lncli unlock
```

When a process exits you will now get a notification sent via email and can login to unlock the wallet and take your node fully online again.


## C-Lightning
To-do: add a clightning install guide

## Eclair

To-do: add an Eclair install guide

