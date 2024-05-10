# Description

This directory contains config files and scripts to reproduce the [issue](https://github.com/envoyproxy/envoy/issues/10076) 
that envoy returns `too large payload` when lua filter is used


# Run tests

   mkdir test_out
   ./lua-filter-buffering/run-test -o test_out

# Run tests manually
## Run backend

    ./lua-filter-buffering/reservation_server

## Run envoy proxies
    
    ./envoy -c ./lua-filter-buffering/envoy.yaml --base-id 1

## Upload file

Create a file

```
dd if=/dev/zero of=bigfile bs=1M count=10
```

```
curl -T bigfile http://127.0.0.1:6080/upload/bigfile
```

You will see an error like below when you hit the issue

```
------------------------------------------------------------
curl output:

Payload Too Large
------------------------------------------------------------
```
