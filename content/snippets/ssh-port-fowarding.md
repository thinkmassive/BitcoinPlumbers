---
title: "SSH Port Fowarding"
draft: false
---

## Local port forwarding
```bash
# expose dest-server:80 (via tunnel through ssh.example.com) on my localhost:8000
ssh -L 8000:dest-server:80 ssh.example.com
```

## Remote port forwarding
```bash
# expose my localhost:80 on the ssh server (ssh.example.com) on its port 80
ssh -R 8000:localhost:80 ssh.example.com
```
