# gocodewalker

[![Go Report Card](https://goreportcard.com/badge/github.com/boyter/gocodewalker)](https://goreportcard.com/report/github.com/boyter/gocodewalker)
[![Str Count Badge](https://sloc.xyz/github/boyter/gocodewalker/)](https://github.com/boyter/gocodewalker/)

Library to help with walking of code directories in Go

NB this was moved from go-code-walker due to the name being annoying and to ensure it has a unique package name. Should still be drop in replaceable
so long as you refer to the new package name.

https://pkg.go.dev/github.com/boyter/gocodewalker

Package provides file operations specific to code repositories such as walking the file tree obeying .ignore and .gitignore files
or looking for the root directory assuming already in a git project.

Example of usage,

```
fileListQueue := make(chan *gocodewalker.File, 100)

fileWalker := gocodewalker.NewFileWalker(".", fileListQueue)
fileWalker.AllowListExtensions = append(fileWalker.AllowListExtensions, "go")

// handle the errors by printing them out and then ignore
errorHandler := func(e error) bool {
    fmt.Println("ERR", e.Error())
    return true
}
fileWalker.SetErrorHandler(errorHandler)

go fileWalker.Start()

for f := range fileListQueue {
    fmt.Println(f.Location)
}
```

The above by default will recursively add files to the fileListQueue respecting both .ignore and .gitignore files if found, and
only adding files with the go extension into the queue.

All code is dual-licenced as either MIT or Unlicence.
Note that as an Australian I cannot put this into the public domain, hence the choice most liberal licences I can find.

### Package

Packaging is done through https://goreleaser.com/ 

### Testing

Done through unit/integration tests. Otherwise see https://github.com/svent/gitignore-test

See `./cmd/gocodewalker/main.go` for an example of how to implement and validate 

### Info

Details on how gitignores work

https://stackoverflow.com/questions/71735516/proper-way-to-setup-multiple-gitignore-files-in-nested-folders-of-a-repository