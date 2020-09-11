## thread


```ruby

th  =  Thread.new{
  # 何か別スレッドの処理
}


# ここで同期
th.join
```