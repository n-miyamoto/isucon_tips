## 静的ファイル配信

以下のようにrootingする

```
location /image/ {
  root /public;
}
```

## リクエストごとのapp server振り分け
location ごとにこんな感じで書く


```
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:3000;
}

location /api/condition/ {
    proxy_set_header Host $http_host;
    proxy_pass http://10.146.226.215:3000;
}
```

## クライアント側でのキャッシュ設定

expire 1dを設定する

```
location /image/ {
  root /public;
  expires 1d;
}
```