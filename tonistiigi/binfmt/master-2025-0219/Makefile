.DEFAULT_GOAL := image-loongarch64
.PHONY: all
.PHONY: image-all
REGISTRY        ?=lcr.loongnix.cn
ORGANIZATION    ?=tonistiigi
REPOSITORY      ?=binfmt
TAG             ?=latest
LATEST          ?=false

IMAGE=$(REGISTRY)/$(ORGANIZATION)/$(REPOSITORY):$(TAG)
LATEST_IMAGE=$(REGISTRY)/$(ORGANIZATION)/$(REPOSITORY):latest
GIT_URL=https://github.com/Loongson-Cloud-Community/binfmt.git

# 架构对应分支
BRANCH_loongarch64=loong64-master-2025-0219

default: image-all

define image

.PHONY: docker-build-list-$(1)

docker-build-list += image-$(1)

image-$(1): src-$(1)
	cd src-$(1) &&  docker buildx build -f Dockerfile --tag "lcr.loongnix.cn/tonistiigi/binfmt:latest" --build-arg HTTPS_PROXY="${https_proxy}" --platform linux/loong64 --load .

src-$(1):
	git clone -b $$(BRANCH_$(1)) $(GIT_URL) $$@

endef

# 定义架构目标
$(eval $(call image,loongarch64))

# 定义 image 目标
image: image-loongarch64
#image:
#	 docker buildx build -f Dockerfile --tag "lcr.loongnix.cn/tonistiigi/binfmt:latest" --build-arg HTTPS_PROXY="${https_proxy}" --platform linux/loong64 --load .


image-all: $(docker-build-list)

# Push 和清理规则
push:
	docker push $(IMAGE)

clean:
	rm -rf src*
