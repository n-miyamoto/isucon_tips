## pprofによるprofiling

pprof を import
```
import (
    ...

	_ "net/http/pprof"

    ...
)
```

echo とかであればこれでOK. go-chiであれば
以下のようなend pointの追加が必要

```go
r.HandleFunc("/pprof/*", pprof.Index)
r.HandleFunc("/pprof/cmdline", pprof.Cmdline)
r.HandleFunc("/pprof/profile", pprof.Profile)
r.HandleFunc("/pprof/symbol", pprof.Symbol)
r.HandleFunc("/pprof/trace", pprof.Trace)

r.Handle("/pprof/goroutine", pprof.Handler("goroutine"))
r.Handle("/pprof/threadcreate", pprof.Handler("threadcreate"))
r.Handle("/pprof/mutex", pprof.Handler("mutex"))
r.Handle("/pprof/heap", pprof.Handler("heap"))
r.Handle("/pprof/block", pprof.Handler("block"))
r.Handle("/pprof/allocs", pprof.Handler("allocs"))
```

## goroutineによる非同期実行

goroutine を用いて非同期に実行できる。
`sync.WaitGroup` を設定して、　`wg.Wait()` で待つことができる。


```go
import (
    "fmt"
    "sync"
)

func goroutine(s string, wg *sync.WaitGroup) {
    for i := 0; i < 5; i++ {
        fmt.Println(s)
    }
    wg.Done()
}
func hoge(s string) {
    for i := 0; i < 5; i++ {
        fmt.Println(s)
    }
}

func main() {
    var wg sync.WaitGroup
    wg.Add(1)
    go goroutine("world", &wg)
    hoge("hello")
    wg.Wait()
}
```

非同期で実行させて、戻り値を得たい場合はchannelを使用する

```go
package main

import "fmt"

func main() {
    ch := make(chan int)                   
    go func() {
        ch <- 5
    }()

    //なにか平行で実施したい処理

    //ここで待ち合わせ. channelから受信
    fmt.Println(<-ch)
}
```


## mapの使い方

初期化
```go
users_cache map[int]User = map[int]User{}
```

アクセスと存在の確認
```go
v, ok := users_cache[p.UserID]
```
okの`true` , `false` で存在の確認
`true` で存在

上書き
```go
sers_cache[p.UserID] = p.User
```

## 排他制御　mutex

mutexの宣言
```go
var mu sync.Mutex
```

lock, unlock
```go
mu.Lock()
なにかcritical sectionな処理
mu.Unlock()
```

## goの簡易コード実行

https://go.dev/play/
