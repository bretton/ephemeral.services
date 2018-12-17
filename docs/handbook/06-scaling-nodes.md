# Scaling nodes

To-do: add info about associating a specific node with a customer as part of round-robin selection when choosing from pool of nodes to issue invoice

The following is relevant for this chapter, and needs to expanded on:

https://www.scipioerp.com/2018/12/12/is-lightning-ready-for-commerce/

```
However, actual transaction speed is only one piece of the equation.  As payment performance also needs to be reliable, merchants will want to also reduce dependence on single Lightning installations and therefore distribute their own traffic to multiple servers. Clustering, running several servers redundantly, is a cornerstone strategy for successful online businesses. The problem: Lightning does not really implement the means to allow for this kind of distributed setup. Lightning, or in this case the LND implementation, only allows incoming connections from a pre-configured list of IPs. In fact, the system is pretty much setup to run on the same server, i.e. allow the commerce application, bitcoin and lightning nodes to share the same server resources. There are ways to connect from an outside system, but setting it up is tricky and dynamic IPs won’t be an option. The competitor c-lightning shares a similar design. As a result, clustering, in particular in a dynamic cloud setup, is very hard and would require a lot of workarounds and configuration management.

Of course we should keep in mind that Lightning is still in its early stages, so I want to be less critical. But because of how Lightning currently is implemented, scaling the system will have its limits. Without the ability to scale, Lightning will remain unattractive for larger installations.
```

and

```
Lightning is not exactly light-weight. You will require a Bitcoin node and a Lightning node and both should ideally be installed on the same server as your business application. All of which will requires a lot of time to setup and configure. Bitcoin will need ~10 days to synchronize the 250GB package (the blockchain) to operate. If one of the systems fails, your business application will not be able to handle cryptocurrency transactions. This is even more problematic, as redundacy cannot be easily achieved (see above).
```

and user comments like

```
Tom Mornini: It’s trivial, and far more reliable, to set up multiple completely independent nodes and randomly distribute the traffic across them.
```

