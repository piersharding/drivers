REGISTRY ?= gitlab.catalyst.net.nz:4567
REGISTRY_NAME=$(REGISTRY)/piers/k8s-hack
DOCKER_USER ?= user
DOCKER_PASSWORD ?= pass
REG_EMAIL ?= user@gitlab.com
KUBE_NAMESPACE ?= default
PULL_SECRET = "gitlab-registry"
GITLAB_USER = piers
CI_BUILD_TOKEN = nmNYaMBh2KvxH6UhEmYf
IMAGE_VERSION=canary
IMG = csi-beegfs-flexdriver
TAG ?= 00.01
DOCKER_IMAGE = $(IMG):$(TAG)
IMAGE_TAG=$(REGISTRY_NAME)/$(DOCKER_IMAGE)
DOCKERFILE ?= Dockerfile
DOCKER = docker
CURRENT_DIR = $(shell pwd)

# define overides for above variables in here
-include PrivateRules.mak

.PHONY: all 

all: build push

build:
	cp driver/beegfs dockerfile/beegfs
	cp ../../../../_output/flexadapter dockerfile/flexadapter
	cd dockerfile && $(DOCKER) build -t $(DOCKER_IMAGE) -f $(DOCKERFILE) .

push:
	$(DOCKER) tag $(DOCKER_IMAGE) $(IMAGE_TAG)
	$(DOCKER) push $(IMAGE_TAG)

namespace:
	kubectl describe namespace $(KUBE_NAMESPACE) || kubectl create namespace $(KUBE_NAMESPACE)

regisry-creds: namespace
	kubectl create secret -n $(KUBE_NAMESPACE) \
	  docker-registry $(PULL_SECRET) \
	 --docker-server=$(REGISTRY) \
	 --docker-username=$(DOCKER_USER) \
	 --docker-password=$(DOCKER_PASSWORD) \
	 --docker-email=$(REG_EMAIL) \
	-o yaml --dry-run | kubectl replace -n $(KUBE_NAMESPACE) --force -f -
