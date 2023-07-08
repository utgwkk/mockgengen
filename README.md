# mockgengen
Generate go:generate comment for mockgen

## Installation

```
go install github.com/utgwkk/mockgengen/cmd/mockgengen@latest
```

## Usage

```go
// foo/foo.go
package foo

import "context"

type IFoo interface {
	Do(ctx context.Context) error
}

type IBar interface {
	Do(ctx context.Context) error
}

var Iset = []any{
	new(IFoo),
	new(IBar),
}
```

```
$ mockgengen -use_go_run -package mock_foo -out ./foo/mockgen.go ./foo Iset
```

```go
// foo/mockgen.go
package fixtures
// Code generated by mockgengen. DO NOT EDIT.

//go:generate go run go.uber.org/mock/mockgen -destination mock_foo/mock_foo.go -package mock_foo . IFoo,IBar
```

You can also use mockgengen with `//go:generate` comment.

```go
package foo

//go:generate go run github.com/utgwkk/mockgengen/cmd/mockgengen -package mock_foo -out ./mockgen.go . Iset

var Iset = []any{
	new(IFoo),
	new(IBar),
}
```

## Restriction

- Mockgengen is available for gomock's reflect mode. Source mode is currently not available.

## Migrate from mockgen

There is a migration tool `mockgen-to-mockgengen`. You can rewrite `//go:generate mockgen` comments to mockgengen's all at once.

```
go install github.com/utgwkk/mockgengen/cmd/mockgen-to-mockgengen@latest
```
