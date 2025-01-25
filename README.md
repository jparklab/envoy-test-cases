# Envoy test cases

This is a repository to store configurations, and scripts to run various tests using Envoy proxy


## http2-fail-with-large-payload

This reproduces the failure that happens when a client uploads a large data to a slow server via two layers of Envoy proxies.
See README.md in the directory for instructions

## strict-dns-cluster

This test is to investigate the behavior of envoy with static dns cluster when a DNS name is updated to have different IP(s)