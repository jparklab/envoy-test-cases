# Config files to reproduce issues with large payload

## Run tests

   ./http2-fail-with-large-payload/run-test

## Run tests manually
### Run backend

    nginx -p ./run -c ../http2-fail-with-large-payload/nginx.conf

### Run envoy proxies
    
    ./envoy-v1.23.2 -c ./http2-fail-with-large-payload/front_envoy.yaml --base-id 1 --component-log-level http2:debug

    ./envoy-v1.23.2 -c ./http2-fail-with-large-payload/back_envoy.yaml --base-id 2 --component-log-level http2:debug

### Upload file

Create a file

```
dd if=/dev/zero of=bigfile bs=1M count=1000
```

```
curl -T bigfile http://127.0.0.1:6080/upload/bigfile
```