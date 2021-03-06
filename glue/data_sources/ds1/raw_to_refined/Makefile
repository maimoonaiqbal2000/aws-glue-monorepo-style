BASEDIR=$(CURDIR)
MK_PATH:=$(dir $(realpath $(lastword $(MAKEFILE_LIST))))
MK_PARENT:=$(realpath $(MK_PATH)../)
JOB_NAME:=$(notdir $(MK_PARENT))
TRANSITION_STATE:=$(notdir $(BASEDIR))
TRANSITION_STATE_PY=$(TRANSITION_STATE).py
JOB_TRANSITION_ZIP=$(BASEDIR)/$(TRANSITION_STATE).zip
BUILD_DIR=$(BASEDIR)/dist


.PHONY: help
help:		## This help.
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

clean: clean-build clean-pyc clean-test ## remove all build, test, coverage and Python artifacts

clean-build: ## Remove build artifacts
	rm -fr build/
	rm -fr dist/
	rm -fr .eggs/
	find . -name '*.egg-info' -exec rm -fr {} +
	find . -name '*.egg' -exec rm -f {} +

clean-pyc: ## Remove Python file artifacts
	find . -name '*.pyc' -exec rm -f {} +
	find . -name '*.pyo' -exec rm -f {} +
	find . -name '*~' -exec rm -f {} +
	find . -name '__pycache__' -exec rm -fr {} +

clean-test: ## Remove test and coverage artifacts
	rm -fr .tox/
	rm -f .coverage
	rm -fr htmlcov/
	rm -fr .pytest_cache

test:  ## Run tests
	echo Tests are not implemented

package: ## Build deps package
	@echo Packaging
	@mkdir -p $(BUILD_DIR)
	@pip install wheel
	cp config.py $(BUILD_DIR)
	pip wheel -w $(BUILD_DIR) -r requirements.txt --no-deps

upload-job: ## Upload job.py file
	@echo Uploading $(JOB_NAME)
	aws s3 cp $(TRANSITION_STATE_PY) s3://your-awsglue-bucket/code/$(JOB_NAME)/$(TRANSITION_STATE)/$(TRANSITION_STATE_PY)

upload: upload-job ## Upload artefacts to S3
	aws s3 sync --delete $(BUILD_DIR) s3://your-awsglue-bucket/code/$(JOB_NAME)/$(TRANSITION_STATE)/dependencies

deploy: clean package upload ## Package and upload to S3
