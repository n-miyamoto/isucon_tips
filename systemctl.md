### サービス起動	
```
systemctl start ${Unit}
```

### サービス停止
```
	systemctl stop ${Unit}
```
### サービス再起動
```
	systemctl restart ${Unit}
```
### サービスリロード
```
	systemctl reload ${Unit}
```
### サービスステータス表示
```
	systemctl status ${Unit}
```
### サービス自動起動有効
```
	systemctl enable ${Unit}
```
### サービス自動起動無効
```
	systemctl disable ${Unit}
```
### サービス自動起動設定確認
```
	systemctl is-enabled ${Unit}
```
###　サービス一覧
```
	systemctl list-unit-files --type=service
```
###　設定ファイルの再読込
```
	systemctl daemon-reload
```