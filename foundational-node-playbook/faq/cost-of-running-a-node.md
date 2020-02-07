# Cost of Running a Node

## Overview

Harmony is using t3.small instances \(2 cores, 2GB RAM, roughly $15.23 per month\) along with 64GB of storage \(standard EBS storage, roughly $6.4 per month\) and 100MB bandwidth.

We're finding recently that actually our greatest cost is becoming the network data since we are sending so much data around the network. I need to make a more accurate measurement of that cost, but the current estimate from our bill is ~$45 per node per month.

### Cost Per Month

* AWS M5A Large ~ $100 per month \(highest performance\)
* AWS t3.small ~$22 per month \(minimal requirement\)
* Vultr High Frequency 128 gb ~$24 per month \(high performance\)
* Vultr Cloud Compute 80 gb ~$20 per month \(good performance\)

### Specification

2 vCPU \(core\), 2G RAM, 60 GB per half year, 100M+ networking bandwidth

### Cost Breakdown per Month

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Cloud</p>
        <p>Provider</p>
      </th>
      <th style="text-align:left">Instance Type</th>
      <th style="text-align:left">Machine Costs
        <br />(on-demand)</th>
      <th style="text-align:left">Machine Cost
        <br />1-year reserved</th>
      <th style="text-align:left">Data Transfer
        <br />Costs</th>
      <th style="text-align:left">Storage</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>| [AWS](https://www.ec2instances.info/?min_memory=2&min_vcpus=1&cost_duration=monthly) | m5a.large | ~60 | ~40 | ~50 | 1 / GB |
| :--- | :--- | :--- | :--- | :--- | :--- |


| [AWS](https://www.ec2instances.info/?min_memory=2&min_vcpus=1&cost_duration=monthly) | t3.small | ~15 | ~9.5 | ~50 | 1 / GB |
| :--- | :--- | :--- | :--- | :--- | :--- |


| [Vultr](https://www.vultr.com/products/high-frequency-compute/) | High Frequency | ~24 | ~24 | 0 | 128 GB Included |
| :--- | :--- | :--- | :--- | :--- | :--- |


| [Vultr](https://www.vultr.com/products/cloud-compute/) | Cloud Compute | ~20 | ~20 | 0 | 80 GB Included |
| :--- | :--- | :--- | :--- | :--- | :--- |


Harmony is reviewing best practices to reduce both data storage and transfer costs.

