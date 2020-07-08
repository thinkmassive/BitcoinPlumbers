---
title: "Bitcoind Address Dump"
draft: false
---

This script displays all non-null output addresses from a single block. It optionally accepts a single parameter, which is the block number to use, otherwise it uses the latest block. Does not do any sorting or filtering.

```bash
# Look up the latest block if none is specified
block_head="$(bitcoin-cli getblockchaininfo | jq .blocks)"
block_num="${1:-$block_head}"

# Get list of all transaction IDs in block
block_hash="$(bitcoin-cli getblockhash $block_num)"
block_txids="$(bitcoin-cli getblock $block_hash | jq -cr .tx | sed 's/,/\n/g' | tr -d '",[]')"

# Print the output address(es) of each transaction
for tx in $block_txids; do
  bitcoin-cli getrawtransaction $tx true | jq -c '.vout[].scriptPubKey.addresses' | sed 's/,/\n/g' | tr -d '",[]' | grep -ve '^null$'
done
```
----

Recommended batch usage:
```bash
start_block=600000
end_block=600100
for block in $(seq $start_block $end_block); do
  ./address_dump.sh $block | sort | uniq >> addresses_${start_block}-${end_block}.txt
done
sort addresses_${start_block}-${end_block}.txt | uniq > addresses.txt
```
----

Recommended blocknotify usage:
```bash
bitcoind -blocknotify="./address_dump.sh | sort | uniq >> addresses.txt" ...
```
----

Note: This script requires jq for JSON parsing. You may want to confirm its availability at the begging of your script:
```bash
[ "$(jq --version &>/dev/null ; echo $?)" != 0 ] && echo jq not found && exit 1
```
