## About

This is a memcache client library for the Go programming language
(http://golang.org/).

It is a fork of https://github.com/bradfitz/gomemcache/memcache,
with support for Autodiscovery client based on https://cloud.google.com/memorystore/docs/memcached/auto-discovery-overview


## Installing

### Using *go get*

    $ go get github.com/google/gomemcache/memcache

After this command *gomemcache* is ready to use. Its source will be in:

    $GOPATH/src/github.com/google/gomemcache/memcache

## Example without Autodiscovery

    import (
            "fmt"
            "github.com/google/gomemcache/memcache"
    )

    func main() {
         // Create a client by providing the list of memcached servers.
         mc := memcache.New("10.0.0.1:11211", "10.0.0.2:11211", "10.0.0.3:11212")

         // For data queries (e.g., SET/GET), the client will first select a
         // memcached server based on computed hash of the key and then send
         // the query to that server.
         mc.Set(&memcache.Item{Key: "foo", Value: []byte("my value")})

         it, _ := mc.Get("foo")
         fmt.Println(string(it.Value))
    }

## Example with Autodiscovery

    import (
            "fmt"
            "time"
            "github.com/google/gomemcache/memcache"
    )

    func main() {
         // Create a client by providing the autodiscovery endpoint. Behind the
         // scene, the client queries the autodiscovery endpoint to fetch the
         // list of memcached servers.
         mcDiscovery, _ := memcache.NewDiscoveryClient("10.0.0.1:11211", 30 * time.Second)

         // For data queries (e.g., SET/GET), the client will first select a
         // memcached server based on computed hash of the key and then send
         // the query to that server.
         mcDiscovery.Set(&memcache.Item{Key: "foo", Value: []byte("my value")})

         it, _ := mcDiscovery.Get("foo")
         fmt.Println(string(it.Value))
         mcDiscovery.StopPolling()
    }

### Note
Remember to call StopPolling() on the discovery enabled client to stop the polling, else this can leak "go" methods.

## Full docs, see:

See https://godoc.org/github.com/google/gomemcache/memcache

Or run:

    $ godoc github.com/google/gomemcache/memcache

