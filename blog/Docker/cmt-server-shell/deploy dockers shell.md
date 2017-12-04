```shell
    As mentioned, Shipyard uses RethinkDB for the datastore. First we will launch a RethinkDB container.
    $> docker run \
        -ti \
        -d \
        --restart=always \
        --name shipyard-rethinkdb \
        rethinkdb
    
    Datastore
    As mentioned, Shipyard uses RethinkDB for the datastore. First we will launch a RethinkDB container.
    
    $> docker run \
        -ti \
        -d \
        --restart=always \
        --name shipyard-rethinkdb \
        rethinkdb
    
    
    #Discovery
    #To enable Swarm leader election, we must use an external key value store from the Swarm container. For this example, we will use etcd however, you can use any key/value backend supported by Swarm.
    
    
    
    $> docker run -ti \
        -d \
        -p 4001:4001 \
        -p 7001:7001 \
        --restart=always \
        --name shipyard-discovery \
        microbox/etcd -name discovery
    
    Proxy
    By default, the Docker Engine only listens on a socket. We could re-configure the Engine to use TLS or you can use a proxy container. This is a very lightweight container that simply forwards requests from TCP to the Unix socket that Docker listens on.
    
    Note: You do not need this if you are using a manual TCP / TLS configuration.
    
    $> docker run \
        -ti \
        -d \
        -p 2375:2375 \
        --hostname=$HOSTNAME \
        --restart=always \
        --name shipyard-proxy \
        -v /var/run/docker.sock:/var/run/docker.sock \
        -e PORT=2375 \
        shipyard/docker-proxy:latest
    
    Swarm Manager
    This will run a Swarm container configured to manage.
    
    $> docker run \
        -ti \
        -d \
        --restart=always \
        --name shipyard-swarm-manager \
        swarm:latest \
        manage --host tcp://0.0.0.0:3375 etcd://127.0.0.1:4001
    
    Swarm Agent
    This runs a Swarm agent that allows the node to schedule containers.
    
    $> docker run \
        -ti \
        -d \
        --restart=always \
        --name shipyard-swarm-agent \
        swarm:latest \
        join --addr <ip-of-host>:2375 etcd://127.0.0.1:4001
    
    Controller
    This runs the Shipyard Controller.
    
    $> docker run \
        -ti \
        -d \
        --restart=always \
        --name shipyard-controller \
        --link shipyard-rethinkdb:rethinkdb \
        --link shipyard-swarm-manager:swarm \
        -p 8080:8080 \
        shipyard/shipyard:latest \
        server \
        -d tcp://swarm:3375
```

``` shell

    docker run -ti \
            -d \
            -p 4001:4001 \
            -p 7001:7001 \
            --restart=always \
            --name shipyard-discovery \
            microbox/etcd -name discovery &&
    docker run \
             -ti \
             -d \
             -p 2375:2375 \
             --hostname=$HOSTNAME \
             --restart=always \
             --name shipyard-proxy \
             -v /var/run/docker.sock:/var/run/docker.sock \
             -e PORT=2375 \
             shipyard/docker-proxy:latest &&
    docker run \
             -ti \
             -d \
             --restart=always \
             --name shipyard-swarm-manager \
             swarm:latest \
             manage --host tcp://0.0.0.0:3375 etcd://127.0.0.1:4001 &&
    docker run \
            -ti \
            -d \
            --restart=always \
            --name shipyard-swarm-agent \
            swarm:latest \
            join --addr 0.0.0.0:2375 etcd://127.0.0.1:4001 &&
    docker run \
             -ti \
             -d \
             --restart=always \
             --name shipyard-controller \
             --link shipyard-rethinkdb:rethinkdb \
             --link shipyard-swarm-manager:swarm \
             -p 8080:8080 \
             shipyard/shipyard:latest \
             server \
             -d tcp://swarm:3375
```

