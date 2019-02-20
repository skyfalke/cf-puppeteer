# CF-puppeteer  


*cf plugin for hands-off, zero downtime application deploys*

## notice

Project forked from [contraband](https://github.com/contraband/autopilot).
Project was updated and renamed to cf-puppeteer to differ the two projects.

# changelog
to give  a better overview i created [changelog](CHANGELOG.md) to show all changes and new features.

Switched from go dep to govendor

## version
The newes version is the 0.0.10 and worked and based on cf-cli version 6.42.0

to get more informations about the newest release see the [Changelog](CHANGELOG.md)

[cf-resource]: https://github.com/concourse/cf-resource

## cf installation

Download the latest version from the [releases][releases] page and make it executable.

```
$ cf install-plugin path/to/downloaded/binary
```

[releases]: https://github.com/happytobi/cf-puppeteer/releases

## usage

```
$ cf zero-downtime-push application-to-replace \
    -f path/to/new_manifest.yml \
    -p path/to/new/path
    -t 120
```
or without application name
```
$ cf zero-downtime-push \
    -f path/to/new_manifest.yml \
    -p path/to/new/path
    -t 120
```

## method

*CF-Puppeteer* takes a different approach to other zero-downtime plugins. It
doesn't perform any [complex route re-mappings][indiana-jones] instead it leans
on the manifest feature of the Cloud Foundry CLI. The method also has the
advantage of treating a manifest as the source of truth and will converge the
state of the system towards that. This makes the plugin ideal for continuous
delivery environments.

1. The old application is renamed to `<APP-NAME>-venerable`. It keeps its old route
   mappings and this change is invisible to users.

2. The new application is pushed to `<APP-NAME>` (assuming that the name has
   not been changed in the manifest). It binds to the same routes as the old
   application (due to them being defined in the manifest) and traffic begins to
   be load-balanced between the two applications.

3. The old application is deleted along with its route mappings. All traffic
   now goes to the new application.

[indiana-jones]: https://www.youtube.com/watch?v=0gU35Tgtlmg


## local development
for local development you need to install [govendor](https://github.com/kardianos/govendor)