# Determining node size/value

According to [Alex Bosworth, to-do: source] it's better for routing nodes to have $40 to $100 channel sizes. This allows routing to be more effective, with fewer paths, than a network with too many tiny channels. 

Transaction fees also need to be factored in, and these can range from epic all time highs, to slight delays on a fuller mempool.

## Fee Considerations

The average fee for a LN channel opening from a Bech32 address in December 2018 is 0.00002787 BTC

## Minimum Viable Channel Size

The minimum viable channel size is equal to (transaction fee 1% reserve of channel size + 1 sat)

For our purposes, at current rates, $100 channel would cost 0.00002787 BTC to open/close, and require 0.03 btc, of which 1% of Z would be required for transaction fees.

## Ideal Channel Sizes

To-do: add more info on why $40-$100 channels are better?

We want out connections to the network to be large to act as routing nodes.

We want our direct customer connections to us be large'ish ($20-$50)

### Wallet Users
To-do: add info on wallet users opening channels to us, such as minimum incoming channel we will allow, which should be less than our outgoing channels?

### Routing Nodes
To-do: we'd like to act as a routing node, with regular submarine swaps to onchain addresses, which gives us lots of incoming capacity on our channels.
