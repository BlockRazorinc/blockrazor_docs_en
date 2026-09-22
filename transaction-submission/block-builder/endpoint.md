---
description: >-
  Find BlockRazor BSC Block Builder endpoints, compare default access, regional
  optimization and fast submit.
metaLinks:
  canonical: endpoint.md
---

# BSC Block Builder Endpoint

### **Default Access**

Prioritize using a global, universal entry point, suitable for quickly completing integration and serving global requests. Endpoint: <mark style="color:$primary;">**https://rpc.blockrazor.builders**</mark>

### **Regional optimization**

If your bot or service is already deployed in a specific region and is more sensitive to latency and consistency, you can further connect to a regional entry point on top of connecting to a globally universal endpoint.

<table><thead><tr><th width="127">Region</th><th width="190.97265625">Available area（AWS）</th><th>RPC Endpoint</th></tr></thead><tbody><tr><td>Tokyo</td><td>apne1-az4</td><td>https://tokyo.builder.blockrazor.io</td></tr><tr><td>Frankfurt</td><td>euc1-az2</td><td>https://frankfurt.builder.blockrazor.io</td></tr><tr><td>Virginia</td><td>use1-az4</td><td>https://virginia.builder.blockrazor.io</td></tr><tr><td>Dublin</td><td>euw1-az1</td><td>https://dublin.builder.blockrazor.io</td></tr></tbody></table>

### **Quality Enhancement**

If you have already completed the basic integration and are starting to focus on the additional overhead in the commit path, cross-region fluctuations, and stability under high load, you can further integrate [Fast Submit](fast-submit.md).
