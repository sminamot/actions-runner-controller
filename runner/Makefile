NAME ?= sminamot/actions-runner

RUNNER_VERSION ?= 2.267.0
DOCKER_VERSION ?= 19.03.8

docker-build:
	docker build --build-arg RUNNER_VERSION=${RUNNER_VERSION} --build-arg DOCKER_VERSION=${DOCKER_VERSION} -t ${NAME}:latest -t ${NAME}:v${RUNNER_VERSION} .

docker-push:
	docker buildx build \
		--build-arg RUNNER_VERSION=${RUNNER_VERSION} \
		--build-arg DOCKER_VERSION=${DOCKER_VERSION} \
		--push \
		--platform linux/arm/v7,linux/amd64 \
		-t ${NAME}:latest \
		-t ${NAME}:${RUNNER_VERSION} .
