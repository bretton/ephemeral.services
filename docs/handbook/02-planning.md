# Planning

We need to plan several steps in advance, after answering some questions:

## How many nodes do we need?

We need multiple nodes, if one goes down for maintenance, the others keep accepting payments. At a minimum, three nodes should suffice.

Our frontend will need to rotate clients between LN nodes on a round-robin or rebalancing basis. LN doesn't handle clustering well.

## How much funding is available for each node?

We want to have at least 10 x $100 channels out. An allocation of $1000 per node for outgoing channels and routing services may be practical, especially during periods of low Bitcoin price. 

## How can each node be optimally funded?

Ideally each node should be funded with 5-10 transactions to separate bech32 addresses. 

The purpose of this is to reduce fees in the long run by making use of segwit addresses, and also to provide a pool of inputs for opening multiple channels simultaneously. 

Because, if we fund with a single transaction, then opening a channel will consume the entire input until 3 confirmations have passed. This makes using this input for a second channel impossible until 3 blocks have confirmed. 

However if we accept an initial 5-10 transaction fee burden, and fund multiple bech32 inputs, we can see better fee scenarios in the future.

# Next steps

Three nodes, with ten channels of $100 out to the network, unknown number of incoming channels. 

Only accept incoming channels of size $40 or more.

$3000 in bitcoin.

