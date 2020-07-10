---
title: "bitcoind"
draft: false
summary: "Bitcoin Core"
git_url: https://github.com/bitcoin/bitcoin
docs_url: https://developer.bitcoin.org/
latest_release:
  version: 0.20.0
  date: 2020-06-03
  url: https://github.com/bitcoin/bitcoin/archive/v0.20.0.tar.gz
  checksum: 
categories:
- FullNodes
tags:
- enterprise
- personal
---

### Links
  - [Git repo](https://github.com/bitcoin/bitcoin)
  - [Developer Guide](https://developer.bitcoin.org/)

### Directory Structure

  - ~/.bitcoin/  340G
    - blocks/    303G
      - index/   100M
    - chainstate/  4G
    - indexes/    31G
      - txindex/  31G
    - wallet.dat  P
    - db.log  P,D
    - debug.log  P,D
    - peers.dat  D

P: may contain private info
D: safe to delete


#### Block Database

The blocks and chainstate dirs contain all data downloaded during the sync process. They are safe to copy between hosts and environments (only from a trusted source).

### Troubleshooting

#### Stopped syncing at a recent block
This should never happen under normal circumstances, but it can be caused by a lower level system issue such as a storage volume fault.

Assuming a safe process restart doesn't resolve the issue, the operation most likely to fix the issue and avoid resyncing from scratch is to start the daemon with `-reindex`

[Pieter Wuille](https://bitcoin.stackexchange.com/questions/22199/what-exactly-did-bitcoin-qt-rescan-reindex-do/22224#22224): -reindex throws away the block chain index and chain state (the database of all unspent transaction outputs), and rebuilds those from scratch. It is exactly like downloading the block chain again from peers, except the blocks already on disk are used. It does not fix corrupted files, but it will detect missing or corrupted ones. In that case the normal block download process from network will start over wherever things went wrong.
