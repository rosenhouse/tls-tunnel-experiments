# mtls inception!

The purpose of this repo is to show that apps can still engage in mutual tls even if traffic is proxied through client-side and server-side sidecars communicating via their own mutual-tls connection.
This shows that mutual tls can be tunneled through another mutual tls connection.

To see this in actions, run the following steps in 4 windows

window 1
```
docker run -it --rm -v ~/go:/go golang /bin/bash
```

window 2, 3, and 4
```
docker exec -it $container_handle /bin/bash
```

then from window 1:
```
cd /go/src/github.com/rosenhouse/tls-tunnel-experiments/
./build

bin/server
```

window 2:
```
cd /go/src/github.com/rosenhouse/tls-tunnel-experiments/

bin/server-proxy
```

window 3:
```
cd /go/src/github.com/rosenhouse/tls-tunnel-experiments/

bin/client-proxy
```

and from window 4:
```
cd /go/src/github.com/rosenhouse/tls-tunnel-experiments/
echo "127.0.0.21    server" >> /etc/hosts

echo "hello" | bin/client -address server:7021
```

you should see that the client is able to reach the server and the server returns a reply.