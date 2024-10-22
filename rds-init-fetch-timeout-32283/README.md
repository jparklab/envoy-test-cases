# Config files to reproduce issue #32283

## Run tests

### Build xds server

We use xds-test-server to send RouteConfiguration and update `init_fetch_timeout`

* Check out and build xds-test-server

    $ git clone https://github.com/jparklab/xds-test-server.git
    $ cd xds-test-server
    $ make

### Run xds server

    # run server
    $ dist/bin/xds-server
    # create a dummy service
    $ ./dist/bin/config-client --action add --num-services 1

### Run envoy proxy

    $ ./envoy -c ./envoy.yaml --base-id 1

### Test connectivity

    # request will be routed to envoy's admin endpoint
    $ curl http://127.0.0.1:10000/ready
    LIVE


### Update initial_fetch_timeout

    $ ./dist/bin/config-client --action set_rds_initial_fetch_timeout --value 10

You will see a message like below from the envoy after 15 seconds, when envoy tries to fetch route configuration and fails due to time out (xds server is not expected to resend route configuration since there is no change in the route configuration)

    [2024-10-22 23:01:37.608][603271][warning][config] [source/common/config/grpc_subscription_impl.cc:120] gRPC config: initial fetch timed out for type.googleapis.com/envoy.config.route.v3.RouteConfiguration

And request will start getting 404 response

    curl -v http://127.0.0.1:10000/ready
    *   Trying 127.0.0.1:10000...
    * TCP_NODELAY set
    * Connected to 127.0.0.1 (127.0.0.1) port 10000 (#0)
    > GET /ready HTTP/1.1
    > Host: 127.0.0.1:10000
    > User-Agent: curl/7.68.0
    > Accept: */*
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 404 Not Found
    < date: Tue, 22 Oct 2024 23:05:24 GMT
    < server: envoy
    < content-length: 0
    < 
    * Connection #0 to host 127.0.0.1 left intact