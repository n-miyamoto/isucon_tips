## pprofによるprofiling
TODO

## goroutineによる非同期実行
TODO

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





