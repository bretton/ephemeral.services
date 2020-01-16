# Firewalling

## Firewall rules for Bitcoin and LN

<del>To-do: add basic firewall rules to allow bitcoin and LN nodes to communicate</del>

Looks like there is a good starting [Alex Bosworth's Run LND documentation]](https://github.com/alexbosworth/run-lnd)

>  Setup a local firewall:
> ```
> sudo ufw logging on
> sudo ufw enable
> # PRESS Y
> sudo ufw status
> sudo ufw allow OpenSSH
> sudo ufw allow 9735
> sudo ufw allow 10009`
> ```

## Rate-limiting

<del>To-do: add ufw rate-limiting rules in pre.rules</del>

Again, from [Alex Bosworth's Run LND documentation]](https://github.com/alexbosworth/run-lnd)

> Setup network flood protection:
> ````
> sudo iptables -N syn_flood
> sudo iptables -A INPUT -p tcp --syn -j syn_flood
> sudo iptables -A syn_flood -m limit --limit 1/s --limit-burst 3 -j RETURN
> sudo iptables -A syn_flood -j DROP
> sudo iptables -A INPUT -p icmp -m limit --limit 1/s --limit-burst 1 -j ACCEPT
> sudo iptables -A INPUT -p icmp -m limit --limit 1/s --limit-burst 1 -j LOG --log-prefix PING-DROP:
> sudo iptables -A INPUT -p icmp -j DROP
> sudo iptables -A OUTPUT -p icmp -j ACCEPT
> ```

To-do: review & update according to own needs