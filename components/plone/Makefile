PATH := .:$(PATH)

.PHONY: bootstrap
bootstrap:
	./.bootstrap.sh
	buildout

.PHONY: buildout
buildout:
	./bin/buildout -N

.PHONY: restart
restart: restart1 restart2

.PHONY: restart1
restart1: stop1 start1

.PHONY: restart2
restart2: stop2 start2

.PHONY: stop1
stop1:
	../../bin/supervisorctl stop instance1

.PHONY: stop2
stop2:
	../../bin/supervisorctl stop instance2

.PHONY: start1
start1:
	../../bin/supervisorctl start instance1
	sleep 30

.PHONY: start2
start2:
	../../bin/supervisorctl start instance2
	sleep 30
