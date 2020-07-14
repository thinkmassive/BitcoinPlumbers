---
title: "ElectrumX"
draft: false
summary: "Electrum server written in python."
repo_url: https://github.com/spesmilo/electrumx
docs_url: https://electrumx-spesmilo.readthedocs.io/
latest_release:
  version: 1.15
  date: 2020-05-27
  url: https://github.com/spesmilo/electrumx/archive/1.15.0.tar.gz
  checksum: 
categories:
- Indexers
tags:
- enterprise
---

Electrum server written in python.

## Troubleshooting

Data corruption typically requires deleting the data and re-initializing. Database repair is theoretically possible ([src](https://github.com/spesmilo/electrumx/issues/41)):
```
cd <datadir>
python3 -c "import plyvel; plyvel.repair_db('hist')"
python3 -c "import plyvel; plyvel.repair_db('utxo')"
```
