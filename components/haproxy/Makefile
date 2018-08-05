PATH := .:$(PATH)

.PHONY: bootstrap
bootstrap:
	./.bootstrap.sh
	buildout

.PHONY: buildout
buildout:
	./bin/buildout -N

restart: stop start

.PHONY: stop
stop:
	../../bin/supervisorctl stop haproxy

.PHONY: start
start:
	../../bin/supervisorctl start haproxy
	sleep 30
