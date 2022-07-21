- [ ] 最初はリクエスト単位の実行時間を計測して全体を俯瞰する。　（内部のdbやその中の内訳は後）
- [ ] まずはリソースの使用状況を見て、サーバの分割を考える。
- [ ] サーバの構成を確認する(各VMのvCPUやメモリ)
- [ ] ミドルウェアのversionや設定を確認する。
  + nginx
  + mysql 
- [ ] 静的ページをnginx で提供する
- [ ] ログの出力で遅くなっていないかを確認する。　（場合によってはログ出力のdisk IOが多い場合があるかも）
- [ ] slow queryの確認とindex. (特に最後の1時間とか。　大きな改善は望めないので、index をはって改善できないか確認する)
- [ ] httpsの場合、設定を考える。（http/2を考える. persistent connection のtimeout時間、非同期通信）
- [ ] 外部APIを確認する　（非同期化する）
