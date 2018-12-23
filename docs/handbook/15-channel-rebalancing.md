# Channel Rebalancing

To-do: needs massive expansion for all the scripts, several possibilities

## Channel Management

Reference:
* [Alex Bosworth - Lightning channel Management](https://www.youtube.com/watch?v=HlPIB6jt6ww)


## Romperts Channel Rebalancer

Some node operators have written their own channel rebalance scripts, like Romperts channel rebalancer

First setup python environment with setup_pythonenv.sh

```
#!/bin/sh
mkdir ~/.pylnd/
cd ~/.pylnd/
pip install grpcio grpcio-tools googleapis-common-protos
git clone https://github.com/googleapis/googleapis.git
curl -o rpc.proto -s https://raw.githubusercontent.com/lightningnetwork/lnd/master/lnrpc/rpc.proto
python -m grpc_tools.protoc --proto_path=googleapis:. --python_out=. --grpc_python_out=. rpc.proto
```

Channel balance script

```
function makeprutt(o) {
    let prutt = {};
    let tlck = parseInt(o.tlck);
    tlck += mytlckextra;
    prutt.total_time_lock = 0;
    prutt.hops = [];
    let lasthop = iam;
    let msat = o.msat;
    for (let l of o.channels) {
        let cc = chaninfo[l];
        console.log('l', l, 'cc', cc);
        let idx = (cc.node2_pub == lasthop) ? 1 : 0;
        let nexthop = idx ? cc.node1_pub : cc.node2_pub;
        let policy = idx ? cc.node2_policy : cc.node1_policy;
        prutt.hops.push({
            chan_id: l,
            expiry: tlck,
            amt_to_forward_msat: msat,
            xpolicy: policy,
        });
        lasthop = nexthop;
    }
    let feemsat = 0;
    let lolmsat = msat;
    let loltlck = tlck;
    for (let i = prutt.hops.length - 1; i >= 0; i--) {
        let h = prutt.hops[i];
        let policy = h.xpolicy;
        delete h.xpolicy;
        console.log('loltlck', loltlck);
        if (i) {}
        h.amt_to_forward_msat = lolmsat.toString();
        h.expiry = loltlck + blockheight;
        h.fee_msat = feemsat.toString();
        feemsat = 0;
        if (i) {
            feemsat = Math.floor(policy.fee_rate_milli_msat * msat / 1000 / 1000 + parseFloat(policy.fee_base_msat));
        }
        loltlck = tlck;
        if (policy)
            tlck += (policy.time_lock_delta || 0);
        lolmsat = msat;
        msat += feemsat;
    }
    prutt.total_time_lock = loltlck + blockheight;
    prutt.total_amt_msat = msat.toString();
    return prutt;
}
```

## Github Rebalance Request

```
https://github.com/lightningnetwork/lnd/issues/1603

It would be really nice to have a Rebalance command with 3 arguments: Outgoing channel ID(optional), Incoming channel ID(optional), and amount.

This would automatically find a route to the next to last hop (you're the last hop), then get all the info it needs for the incoming channel and adds the last route, creates a pay request, and uses sendtoroute to fulfill the request.

If outgoing or incoming channels are not specified, this could be populated a couple ways:
One: find an unbalanced channel(s) that's used the least amount
Two: Find the most unbalanced channel(s).
```

## ln-rebalance

Another project trying to assist with channel rebalancing is https://github.com/grunch/ln-rebalance

## rebalance-lnd

And another project: https://github.com/C-Otto/rebalance-lnd