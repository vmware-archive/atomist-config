# atomist-config-seed

Atomist configuration repository for a GitHub organization.

## Structure

This repository contains configuration used by various aspects of the
Atomist system.  The files in the repository are managed by Atomist,
so you should not need to edit them but you can view them to get an
idea of how Atomist understands your repositories and their
relationships.

### Features

Features represent a unit of functionality in a project.  A feature
could be something like logging, database connectivity, CI
configuration, dependencies, or a framework.

The `features.json` file manages how features flow between
repositories in your organization.  A single project can be a
broadcaster of a features while many projects could be a follower of
that feature in that project.  A given project could be the
broadcaster of many features.  A given project could also
simultaneously be a broadcaster of some features and a follower of
other features.

The following features are currently available as part of the core
offering.

Feature | Description
--------|-----------
`Java.Properties` | additions and removals of property keys and values
`Java.Structure` | additions of non-source-code files to a Java project
`Maven.Dependency` | additions, removals, and updates of dependencies in a [Maven][maven] POM
`Maven.Docker` | changes to a Dockerfile in a Maven project
`Maven.Plugin` | additions, removals, updates of plugins in a Maven POM
`Maven.Travis` | changes to [Travis CI][travis-ci] configuration and build scripts in a Maven project

[maven]: https://maven.apache.org/
[travis-ci]: https://travis-ci.org

An example `features.json` file is below.

```json
{
    "broadcast": {
        "spring-rest-seed": {
            "features": [
                {
                    "name": "Java.Properties",
                    "followers": [
                        "name-service",
                        "address-service",
                        "phone-service"
                    ]
                },
                {
                    "name": "Java.Structure",
                    "followers": [
                        "name-service",
                        "address-service",
                        "phone-service"
                    ]
                },
                {
                    "name": "Maven.Dependency",
                    "followers": [
                        "name-service",
                        "address-service"
                    ]
                },
                {
                    "name": "Maven.Docker",
                    "followers": [
                        "name-service",
                        "address-service"
                    ]
                },
                {
                    "name": "Maven.Plugin",
                    "followers": [
                        "name-service",
                        "address-service"
                    ]
                },
                {
                    "name": "Maven.Travis",
                    "followers": [
                        "name-service",
                        "address-service"
                    ]
                }
            ]
        },
        "scala-lib-seed": {
            "features": [
                {
                    "name": "Maven.Dependency",
                    "followers": [
                        "neo4j-lib",
                        "dynamodb-lib"
                    ]
                },
                {
                    "name": "Maven.Plugin",
                    "followers": [
                        "neo4j-lib",
                        "dynamodb-lib"
                    ]
                },
                {
                    "name": "Maven.Travis",
                    "followers": [
                        "neo4j-lib"
                    ]
                }
            ]
        }
    }
}
```

The top-level `broadcast` element has sub-elements for each broadcast
repository.  Each broadcast repository contains an array of
`features`, each having a `name` and a list of `followers`.  The
`feature` property can also have an optional `config` property whose
contents vary depending on the feature.

### Fingerprints

Fingerprints are simple strings that represent a snapshot of the
semantic structure of some aspect of your project.  For example, you
may wish to have a fingerprint for your public API.  The algorithm for
the fingerprint might read the annotations for methods defining
endpoints and create unique fingerprints for each possible API.  The
value of the fingerprint can be tracked from commit to commit.  If it
changes from one commit to the next, you have an indication that the
public API for your service has changed.

```json
{
    "repos": {
        "spring-rest-seed": [
            "Maven.+",
            "Spring.+"
        ],
        "$default$": [
            "Maven.+",
            "Spring.+"
        ]
    }
}
```
