CWD := $(shell pwd)
TAG := stable
IMAGE := "<%= image %>:$(TAG)"
SERVICE := <%= service %>
SOME_CONTAINER := $(shell echo some-$(IMAGE) | sed 's/[^a-zA-Z0-9]//g')
DOCKERFILE := $(CWD)/$(TAG)/Dockerfile

.PHONY: all
all: clean build

.PHONY: build
build:
	@docker build -t $(IMAGE) -f $(DOCKERFILE) $(CWD)
	@echo ::: BUILD :::

.PHONY: pull
pull:
	@docker pull $(IMAGE)
	@echo ::: PULL :::

.PHONY: push
push:
	@docker push $(IMAGE)
	@echo ::: PUSH :::

.PHONY: info
info:
	@docker inspect -f '{{.Config.Labels}}' $(IMAGE)

.PHONY: up
up:
	@docker-compose -f $(TAG)/docker-compose.yml up --force-recreate

.PHONY: run
run:
	@docker-compose -f $(TAG)/docker-compose.yml run $(SERVICE)

.PHONY: ssh
ssh:
	@export CONTAINER_NAME=$$(docker ps --format '{{.Names}}' | grep -m 1 $(TAG)_$(SERVICE)_) && \
    if [ "$$CONTAINER_NAME" = "" ]; then \
    docker-compose -f $(TAG)/docker-compose.yml run --entrypoint /bin/sh $(SERVICE); else \
    echo "ssh into $$CONTAINER_NAME" && \
    docker exec -it $$CONTAINER_NAME /bin/sh; fi

.PHONY: clean
clean:
	-@ rm -rf ./stuff/to/clean &>/dev/null || true
	@echo ::: CLEAN :::
