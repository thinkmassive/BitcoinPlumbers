---
title: "{{ replace .Name "-" " " | title }}"
draft: false
summary: "Summary of the {{ replace .Name "-" " " | title }} project"
git_url: https://github.com//{{ .Name }}
docs_url: 
categories:
- fullnodes
- wallets
- indexers
- miners
- libraries
- utilities
- text
tags:
---

Description of the {{ replace .Name "-" " " | title }} project...

<h3>Links</h3>
<ul>
  <li><a href="{{ .Params.git_url }}">git repo</a></li>
  <li><a href="{{ .Params.docs_url }}">docs</a></li>
</ul>
