---
layout: post
title: "Debugging helm charts from terraform"
description: ""
tags: ['kubernetes','terraform','helm']
---

<p>
  The best way I've found to debug a helm chart from terraform, is to use the data helm_template terraform resource and to dump the full template out to files.
</p>

<pre>
data "helm_template" "this" {
  name      = "helm-chart"
  namespace = kubernetes_namespace.this.0.metadata.0.name
  version   = local.version

  repository = "https://charts.${your chart domain here}.com"
  chart      = local.chart

  values = [
    local.values
  ]
}

resource "local_file" "this" {
  for_each = data.helm_template.this.manifests

  filename = "./template_debug/${each.key}"
  content  = each.value
}
</pre>

<p>
  Doing this with the "bad" and "good" helm chart installs will show you the full difference of the things that are about to be installed.
  In my case I was switching from a values template file to set blocks within terraform, and had made a typo in my set blocks where one resources limit was being written twice instead of setting a request, and the app was crashing after eating up more memory than the base request asks for.
  Admittedly now that I'm aware that the base request should be higher I will be raising the limit, but this was still a very effective way of seeing where I had went wrong.
  Always go back to the basics, diffing changes has been one of the best ways for me to see where I've gone wrong.
</p>
