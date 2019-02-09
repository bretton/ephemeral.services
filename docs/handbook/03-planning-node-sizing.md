# Determining node size/value

According to [Alex Bosworth, to-do: source] it's better for routing nodes to have $40 to $100 channel sizes. This allows routing to be more effective, with fewer paths, than a network with too many tiny channels. 

Additionally "If you want the range of your payments to be 0-$100, don’t create 10 $20 channels, instead create 1 $200 channel"

Transaction fees also need to be factored in, and these can range from epic all time highs, to slight delays on a fuller mempool.

## Fee Considerations

The average fee for a LN channel opening from a Bech32 address in December 2018 is 0.00002787 BTC

## Minimum Viable Channel Size

The minimum viable channel size is equal to (transaction fee 1% reserve of channel size + 1 sat)

For our purposes, at current rates, $100 channel would cost 0.00002787 BTC to open/close, and require 0.03 btc, of which an additional 1% would be reserved for LN channel transaction fees (close).

## Ideal Channel Sizes

To-do: add more info on why $40-$100 channels are better?

We want our:
* outgoing channels to be sensibly large, and act as routing nodes.($100+)
* incoming connections to be large rather than small. ($75+)
* direct customer channels to be large'ish ($20-$50)

### Wallet Users
To-do: add info on wallet users opening channels to us, such as minimum incoming channel we will allow, which should be less than our outgoing channels?

### Routing Nodes
To-do: we'd like to act as a routing node, with regular submarine swaps to onchain addresses, which gives us lots of incoming capacity on our channels.
