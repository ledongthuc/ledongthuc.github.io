---
title: "Go module"
series: "go"
categories: "notes"
summary: "Notes when using Go Module"
date: 2020-04-04T21:00:00+01:00
draft: true
tags:
- go
- module
- dependencies 
---

# Summary

go build, go test, and other package-building commands add new dependencies to go.mod as needed.

go.mod

 - Define go version that use to run module
 - Define required library version need to query
 - Library with `--indirect` indicates a dependency is not used directly by this module, only indirectly by other module dependencies.

 	

go.sum

 - Is used by go command to ensure that future downloads of these modules retrieve the same bits as the first download
 - Ensure the modules your project depends on do not change
 - Should commit to version control

Caching module
 - $GOPATH/pkg/mod

Pseudo-versions
	
Untagged revisions are used when versionning dependencies

 - vX.0.0-yyyymmddhhmmss-hashcommit is used when there is no earlier versioned commit with an appropriate major version before the target commit
 - vX.Y.Z-pre.0.yyyymmddhhmmss-hashcommit is used when the most recent versioned commit before the target commit is vX.Y.Z-pre
 - vX.Y.(Z+1)-0.yyyymmddhhmmss-hashcommit is used when the most recent versioned commit before the target commit is vX.Y.Z

Converting:

 - https://github.com/golang/go/blob/master/src/cmd/go/internal/modconv/modconv.go

## Commands:

	go mod init <package>
		Init a new module
		Use to convert other dependencies to go modules https://github.com/golang/go/blob/master/src/cmd/go/internal/modconv/modconv.go

	go list -m all
		List all dependencies in a module

	go list -m rsc.io/quote...
		List all dependencies with different version of rsc.io/quote

	go list -m -versions rsc.io/sampler
		List all all version of rsc.io/quote supported

	go mod tidy
		Clean up unused dependencies

# References:

 - https://github.com/golang/go/wiki/Modules
 - https://blog.golang.org/using-go-modules
 - https://semver.org/