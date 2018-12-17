# DNS Tweaks
The following is put together with help from [Tzybit's BOLT12 proposal](https://github.com/tyzbit/lightning-rfc/blob/bolt12-node-dns-advertisement/12-node-dns-advertisement.md) and the author's [guide to obtaining node key in Bech32 format](https://gist.github.com/bretton/7afb35c907b27655a57acc64fdf7a60c).

First we need our node public key in Bech32 format, then we need to add SRV records to our DNS to share relevant information on our LN node.

## Bech32 node address
We are going to make use of the following [Bech32 tool](https://github.com/nym-zone/bech32) to `Encode and decode Bech32 strings and Bitcoin Segwit addresses``

### Installation

To install, clone to a directory as follows:

```
git clone https://github.com/nym-zone/bech32
cd bech32
```

However you will get an error on the documented instructions:

> Input:  
> ```  
> make -f Makefile.linux && make -f Makefile.linux check && sudo make -f Makefile.linux install  
> ```  
> Output:  
> ```  
> make: Nothing to be done for 'all'.  
> make: *** No rule to make target 'check'.  Stop.  
> ```  
  

So instead go with the following which works out fine.

Input:
```
make -f Makefile.linux && sudo make -f Makefile.linux install
```

Output:
```
cc -O2 -std=c99   -c -o bech32.o bech32.c
cc -O2 -std=c99   -c -o segwit_addr.o segwit_addr.c
cc -o bech32 bech32.o segwit_addr.o 
[sudo] password for YOURUSER: 
install bech32 /usr/local/bin
install bech32.1 /usr/local/man/man1
```

Then to encode our LN node's public key to a Bech32 string we'll use:

Outline:
```
bech32 -e -h <human-readable-part> <our-node-pubkey>
```

For the human readable part we're going to use `ln` for Lighting Network. As follows:

Input:
```
bech32 -e -h ln <node-pub-key>
```

Output
```
ln<alphanumerics>
```

We can now do this for our my lightning nodes and update DNS accordingly.

### Useful
Find your node at https://www.robtex.com/lightning/node/ and there is output to check what is expected from DNS, along with validation once it's setup.

There is also a python library here: https://github.com/sipa/bech32/tree/master/ref/python

## SRV records

Be sure to read [BOLT #12: Node Advertisement via DNS Subdomains](https://github.com/tyzbit/lightning-rfc/blob/bolt12-node-dns-advertisement/12-node-dns-advertisement.md) first.

Once we have the generated node key above we can update our DNS to include SRV records for our LN nodes.

Add a SRV record for 

```
_lightning._tcp.ephemeral.services	10 9735 ln<generated-alphanumerics>.ephemeral.services.
```

Reload DNS

## Round-Robin requests

To-do: add info on making use of Round-robin DNS requests to spread load for LN servers over a cluster.