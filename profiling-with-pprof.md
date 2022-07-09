```go
import (
	_ "net/http/pprof"
)
```

run pprof with 

```
# run pprof (in another terminal)
go tool pprof -http=":8888" http://localhost:6060/debug/pprof/profile?seconds=90
```