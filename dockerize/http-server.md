# Containerizing an HTTP server

In this lab you will build an http server, run it locally, package it in a
container, and then deploy it to a Kubernetes cluster.

## Exercise: Build an HTTP server

This can be accomplished in the language of your choice. The examples here are
in Go for brevity.

The first step is to write your code, of course. Here's something that works for
Go:

```go
package main

import (
        "flag"
        "fmt"
        "log"
        "net/http"
)

var port = flag.Int("port", 8080, "Port on which to listen")

func main() {
        flag.Parse()
        http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
                fmt.Fprintln(w, "Hallo.")
        })
        log.Fatal(http.ListenAndServe(fmt.Sprintf(":%d", *port), nil))
}
```

### Building the container

Docker provides a library of official images on which to build your application
at https://hub.docker.com. The official images have various amounts of magic,
and the golang image has maximum magic:

```bash
$ ls
Dockerfile  serve.go
$ cat Dockerfile
FROM golang:onbuild
$ docker build -t gcr.io/<YOUR-PROJECT-ID>/serve .
...
Successfully built 17d9afdfea31
```

Docker uses tags to identify images, and `gcr.io/<YOUR-PROJECT-ID>` is the path
for the Google Container Registry for your project. We can test the image:

```bash
$ docker run --name=serve -p 8080:8080 --detach gcr.io/verb-test/serve
9850e525f3042164b871d93e43bead37a27c156a72eb7271f7f4fe628d049c32
$ curl localhost:8080
Hallo.
$ docker kill serve && docker rm serve
```

At this point you've got plenty of magic to decode. Luckily the magic is fairly
well documented in the official images:

* [Bash](https://hub.docker.com/_/bash/)
* [Django](https://hub.docker.com/_/django/)
* [Go](https://hub.docker.com/_/golang/)
* [Java (OpenJDK)](https://hub.docker.com/_/openjdk/)
* [Jetty](https://hub.docker.com/_/jetty/)
* [nginx](https://hub.docker.com/_/nginx/)
* [Perl](https://hub.docker.com/_/perl/)
* [PHP](https://hub.docker.com/_/php/)
* [Python](https://hub.docker.com/_/python/)
* [Ruby](https://hub.docker.com/_/ruby/)
* [Tomcat](https://hub.docker.com/_/tomcat/)

If you'd like less magic in your life, you can instead write a
[Dockerfile](https://docs.docker.com/engine/reference/builder/) to do a scripted
install on top of an OS base image:

* [Alpine](https://hub.docker.com/_/alpine/)
* [Debian](https://hub.docker.com/_/debian/)
* [Ubuntu](https://hub.docker.com/_/ubuntu/)

If you get stuck try searching for an example on https://hub.docker.com

### Uploading the container to Google Container Registry

`gcloud` has a docker wrapper to handle the authentication required for pushing
an image to Google Container Registry.

```
gcloud docker -- push gcr.io/verb-test/serve
```
