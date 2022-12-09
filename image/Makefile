PROJECT = karota
DISCOURSE_IMAGE = discourse/base
IMAGE_SLIM = karota_base_slim
IMAGE_BASE = karota_base
TAG_SLIM = build_slim
TAG_BASE = build
LATEST_TAG = $(shell git -c 'versionsort.suffix=-' \
    ls-remote --exit-code --refs --tags https://github.com/charnelclamosa/karota_webapp '*.*.*' \
    | tail --lines=1 \
    | cut --delimiter='/' --fields=3)
APP_ENV ?= uat
G_REPO = gcr.io/$(PROJECT)-$(APP_ENV)

build-slim:
	sudo ruby auto_build.rb base_slim

build-base:
	sudo ruby auto_build.rb base

upload-slim:
	docker tag $(DISCOURSE_IMAGE):$(TAG_SLIM) $(G_REPO)/$(IMAGE_SLIM):$(LATEST_TAG)
	docker tag $(DISCOURSE_IMAGE):$(TAG_SLIM) $(G_REPO)/$(IMAGE_SLIM):latest
	docker push $(G_REPO)/$(IMAGE_SLIM):$(LATEST_TAG)
	docker push $(G_REPO)/$(IMAGE_SLIM):latest

upload-base:
	docker tag $(DISCOURSE_IMAGE):$(TAG_BASE) $(G_REPO)/$(IMAGE_BASE):$(LATEST_TAG)
	docker tag $(DISCOURSE_IMAGE):$(TAG_BASE) $(G_REPO)/$(IMAGE_BASE):latest
	docker push $(G_REPO)/$(IMAGE_BASE):$(LATEST_TAG)
	docker push $(G_REPO)/$(IMAGE_BASE):latest

deploy: build-slim upload-slim build-base upload-base