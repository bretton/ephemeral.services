# Introduction

This is the Lightning Network (LN) operational handbook for "ephemeral.services", a fictional entity providing time-limited VPN credentials in exchange for LN payments.

It is intended to be a living document, under regular revision. 

This handbook does not currently address the implementation of VPN solutions, nor a support desk guide, or any other documentation which may be useful for a VPN provider.

Nor does it currently address the coding of payment windows and talking to RADIUS servers.

At this stage the handbook only addresses the sysadmin elements of running several LN nodes. 

## How ephemeral.services works

A person joins any network and accesses https://ephemeral.services in a browser.

They then select an option for VPN credentials to a service, such as a 1 hour pass, or 1 day pass.

They are presented with an invoice they have to pay within 1-60 minutes.

The user scans the QR code to pay, and on payment, is presented with login details to add as a VPN connection with username, password, IP address of server. 

In the backend, a paid invoice triggers allocation and enablement of a user on a radius server. These details are synced across the VPN and presented to the user to make use of. 

## Considerations

It's important to ensure we stay operational under all circumstances. This means having multiple nodes with good capacity, and failover mechanisms. 

The entire billing and operational side of providing a VPN network must be 'bootable' and recoverable within an hour of take-down or denial-of-service attack. This includes funding routing nodes from a cold wallet store and ensuring outgoing capacity on the network.

Support-desk queries can be 1 sat requests.

We need a bird's eye view of the state of our nodes, networks, network providers.

Some servers might need to run for 10 years, so the design choices have to take into consideration that key nodes might not be shutdown for a long time.