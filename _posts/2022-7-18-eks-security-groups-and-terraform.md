---
layout: post
title: "EKS, Security Groups and Terraform"
description: ""
tags: ['kubernetes','eks','terraform','security-groups','securitygroups']
---

<a href="#tldr">TLDR</a>

<p>
<h2>Background</h2>
I use a module in terraform to auto-create nice default settings with eks and aws called <a href="https://github.com/terraform-aws-modules/terraform-aws-eks">terraform-aws-eks</a>.
Upgrading from 17 to 18 was bumpy to say the least.
</p>

<p>
Events:
  Type     Reason        Age                From                   Message
  ----     ------        ----               ----                   -------
  Warning  FailedCreate  5m2s               replicaset-controller  Error creating: Internal error occurred: failed calling webhook "pods-kuma-injector.kuma.io": failed to call webhook: Post "https://kuma-control-plane.kuma-system.svc:443/inject-side
car?timeout=10s": dial tcp 10.0.2.120:5443: i/o timeout
  Warning  FailedCreate  59s (x68 over 3d)  replicaset-controller  Error creating: Internal error occurred: failed calling webhook "pods-kuma-injector.kuma.io": failed to call webhook: Post "https://kuma-control-plane.kuma-system.svc:443/inject-side
car?timeout=10s": context deadline exceeded
</p>

<p>
<h2 id="#tldr">TLDR</h2>
During the upgrade I missed that they had previously allowed the cluster and nodes to communicate freely.
The new security groups were blocking the eks control plane from communicating with services running on the nodes which prevented pods from coming online.
</p>
