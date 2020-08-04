
### install 

```
$ wget https://github.com/tkuchiki/alp/releases/download/v1.0.3/alp_linux_amd64.zip
$ unzip alp_linux_amd64.zip
$ sudo install ./alp /usr/local/bin
```

### profiling 

```
alp ltsv --file="/var/log/nginx/access.log"  -m  '/new_items/*','/items/*','/transactions/*','/upload/*','/users/*'
--sort=sum -r
```

- ltsv : format指定 
- --file="/var/log/nginx/access.log"  file指定
- m: matching pattern指定
- --sort=sum : どのkeyでソートするか
- --format csv : csv出力

### nginx.conf

```
sudo vim /etc/nginx/nginx.conf

http {
  log_format ltsv "time:$time_local"
    "\thost:$remote_addr"
    "\tforwardedfor:$http_x_forwarded_for"
    "\treq:$request"
    "\tmethod:$request_method"
    "\turi:$request_uri"
    "\tstatus:$status"
    "\tsize:$body_bytes_sent"
    "\treferer:$http_referer"
    "\tua:$http_user_agent"
    "\treqtime:$request_time"
    "\truntime:$upstream_http_x_runtime"
    "\tapptime:$upstream_response_time"
    "\tcache:$upstream_http_x_cache"
    "\tvhost:$host";

  access_log  /var/log/nginx/access.log ltsv;
}
```
