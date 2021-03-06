PATH := node_modules/.bin:../../node_modules/.bin:$(PATH)
SHELL := /bin/bash

DOCKER_REPO := graphql

.FORCE:

all: clean .FORCE
	webpack --colors --display-error-details --config config/webpack/server.babel.js

image: shrinkwrap .FORCE
	docker build -t user/repo:test

clean:
	concurrently "rimraf dist"

test: .FORCE
	mocha

lint: .FORCE
	concurrently "eslint src" "eslint test" "eslint config"

dependencies: .FORCE
	yarn

start-server: .FORCE
	just-wait --pattern 'dist/*.js'
	node dist/index

watch: .FORCE
	concurrently \
		"webpack --watch --verbose --colors --display-error-details --config config/webpack/server-dev.babel.js" \
		"make start-server"

package: .FORCE
	docker build -t $(DOCKER_ACCOUNT)/$(DOCKER_REPO):$(DOCKER_TAG) .

deploy: .FORCE
	./scripts/deploy.sh

configure: .FORCE
	direnv allow

environment: .FORCE

seed: .FORCE
	./scripts/seed.sh
