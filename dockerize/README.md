# Dockerize an App

## Prerequisites

* Have docker installed

## Lab

Unless you're already familiar with building containers, complete the [Build an
HTTP server](http-server.md) lab.

## Exercise

Bowerick Wowbagger the Infinitely Prolonged has contracted you to create a web
service to assist him insult everybody in the universe. Due to the relatively
large number of bodies in the universe, you've decided to implement a two-tiered
architecture.

Create a load balanced web service that accepts the name of a being (e.g.
"Arthur Dent"), contacts the backend InsultService to retrieve an appropriate
insult (e.g. "a complete kneebiter") and returns a complete, personalized
insult.

## Relevant Documentation

* [Running a stateless application](https://kubernetes.io/docs/tutorials/stateless-application/run-stateless-application-deployment/)
* [Connecting a front end to a back end](https://kubernetes.io/docs/tutorials/connecting-apps/connecting-frontend-backend/)
* [Service Discovery](https://kubernetes.io/docs/user-guide/services/#discovering-services)

## Cleanup

Don't forget to delete your services, deployments and the cluster itself.
