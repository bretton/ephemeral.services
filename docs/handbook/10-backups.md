# Backups

reference:
https://gist.github.com/bretton/22f628caffde79390a796e75ea528053
https://github.com/lightningnetwork/lnd/pull/2313

reference 2 for LND
```
## Moving Forward on Backups

The new SCB backups feature coming to LND is moving ahead with a
strategy of splitting it up  pieces designed to get it through code
review smoothly.

Quick refresher on the SCB backups feature: it allows you to backup
your channel with a one-time file that you can save somewhere
redundant, and this file never needs updating. The SCB tradeoff is
that to restore your channel funds you will need to reconnect to your
peer, the peer must support the data-loss-protection feature, and the
channel must be force closed.

These pieces:
- "channeldb: prepatory modifications for full SCB implementation P2
backups database" https://github.com/lightningnetwork/lnd/pull/2368
- "multi: implement new lncli and lnrpc commands for SCB's":
https://github.com/lightningnetwork/lnd/pull/2372
- "chanbackup: add new package implementing static channel backups"
https://github.com/lightningnetwork/lnd/pull/2370

This is a great feature because it's fairly simple in scope, but also
solves tons of practical scenarios or mitigates a lot of loss. It may
influence the shape of the network some though. Having unreliable
peers now also means you have unreliable backup partners, so choosing
great peers could become even more important.
```

To-do: update this with the new LND backup procedure once it goes live

## Process

stop application
perform backup of directory
start application
unlock wallet (optionally via API)

> For auto unlocking of wallet see [Balance of Satoshis](https://github.com/alexbosworth/balanceofsatoshis) or build a script to use the API

## Tools

automating hourly backups and node restart & unlock


