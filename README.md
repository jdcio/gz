Gz
============

This is a tiny Go package which wraps HTTP handlers to transparently gzip the
response body, for clients which support it. Although it's usually simpler to
leave that to a reverse proxy (like nginx or Varnish), this package is useful
when that's undesirable.


## Usage

Call `Gz` with any handler (an object which implements the
`http.Handler` interface), and it'll return a new handler which gzips the
response. For example:

```go
package main

import (
	"io"
	"net/http"
	"github.com/jdcio/gz"
)

func main() {
	withoutGz := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "text/plain")
		io.WriteString(w, "Hello, World")
	})

	withGz := gz.Handler(withoutGz)

	http.Handle("/", withGz)
	http.ListenAndServe("0.0.0.0:8000", nil)
}
```
