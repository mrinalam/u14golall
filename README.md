u14golall
=============

Shippable CI image for Go on Ubuntu 14.04 with all services available. Available versions are:

1. 1.1
2. 1.2
3. 1.3
4. 1.4
5. 1.5

**SERVICES**

1. cassandra 2.1
2. elasticsearch 1.5
3. memcached 1.4
4. mongodb 3.0
5. mysql 5.6
6. neo4j 2.2
7. postgres 9.4
8. rabbitmq 3.5
9. redis 3.0
10. selenium 2.45
11. sqllite 3
12. couchdb 1.6
13. rethinkdb 2.0
14. riak latest

## How to use
You can use this image to run go builds on Shippable. Add build_image tag to your shippable.yml file and activate gvm in before_install section to run your build against the correct version of go.  Here's a sample YML file to get you going:

````
language: go
go:
  - 1.1
  - 1.2
  - 1.3
  - 1.4
  - 1.5

services:
  - cassandra
  - elasticsearch
  - memcached
  - mongodb
  - mysql
  - neo4j
  - postgres
  - rabbitmq
  - redis
  - selenium
  - sqllite
  - couchdb
  - rethinkdb
  - riak

build_image: drydock/u14golall:prod

before_install:
  - source $HOME/.gvm/scripts/gvm;
  - if [[ $SHIPPABLE_GO_VERSION == "tip" ]]; then gvm install tip; gvm use tip; fi
  - if [[ $SHIPPABLE_GO_VERSION == *release* ]]; then gvm install release; gvm use release; fi
  - if [[ $SHIPPABLE_GO_VERSION =~ [0-9].[0-9] ]]; then gvm install go$SHIPPABLE_GO_VERSION; gvm use go$SHIPPABLE_GO_VERSION; fi
  - export GOPATH=$SHIPPABLE_GOPATH
  - go get github.com/t-yuki/gocover-cobertura
  - go get github.com/onsi/gomega
  - go get github.com/onsi/ginkgo
  - go get code.google.com/p/go.tools/cmd/cover

install:
  - go get -d -v ./... && go build -v ./...

script:
  - go test -coverprofile=coverage.txt -covermode count ./

````
