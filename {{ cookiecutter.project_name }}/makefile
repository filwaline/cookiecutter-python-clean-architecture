
# these will speed up builds, for docker-compose >= 1.25
export COMPOSE_DOCKER_CLI_BUILD=1
export DOCKER_BUILDKIT=1

compose-all: compose-down compose-build compose-up compose-test

compose-build:
	docker-compose -f "./docker/docker-compose.yml" build

compose-up:
	docker-compose -f "./docker/docker-compose.yml" up -d app

compose-down:
	docker-compose -f "./docker/docker-compose.yml" down --remove-orphans

compose-bash: compose-up
	docker-compose -f "./docker/docker-compose.yml" exec app bash

compose-test: compose-up
	docker-compose -f "./docker/docker-compose.yml" run --rm --no-deps --entrypoint=pytest app /tests/unit /tests/integration /tests/e2e

compose-unit-tests: compose-up
	docker-compose -f "./docker/docker-compose.yml" run --rm --no-deps --entrypoint=pytest app /tests/unit

compose-integration-tests: compose-up
	docker-compose -f "./docker/docker-compose.yml" run --rm --no-deps --entrypoint=pytest app /tests/integration

compose-e2e-tests: compose-up
	docker-compose -f "./docker/docker-compose.yml" run --rm --no-deps --entrypoint=pytest app /tests/e2e

compose-logs:
	docker-compose -f "./docker/docker-compose.yml" logs app | tail -100

black:
	black -l 86 $$(find * -name '*.py')

pre-commit-install:
	pre-commit install

pre-commit-update: pre-commit-install
	pre-commit autoupdate

pre-commit-check-all: pre-commit-install
	pre-commit run --all-files

poetry-self-install:
	curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python

poetry-setup:
	poetry update && poetry install

poetry-test-all: poetry-setup
	poetry run pytest --cov={{ cookiecutter.service_name }} ./tests

poetry-test-unit: poetry-setup
	poetry run pytest --cov={{ cookiecutter.service_name }} ./tests/unit
