SHELL := /bin/bash

include .env
STACK_NAME?=WildRydesIAM

GIT_FQDN?=                   $(shell git config --get remote.origin.url | sed 's@\(.*\):\(.*\).git@\2@g')
GIT_REF_NAME?=               $(shell git rev-parse --abbrev-ref HEAD | tr '/' '-' )
GIT_REF_SHORT?=              $(shell git log -1|head -n1|cut -d ' ' -f2- | head -c 8)

CFN_CHANGE_SET?=             $(GIT_REF_NAME)-$(GIT_REF_SHORT)

PROFILE?=										default

help:           ## prints help
	@cat $(MAKEFILE_LIST) | grep -e "^[a-zA-Z_\-]*: *.*## *" | awk 'BEGIN {FS = ":.*?## "}; {printf " > \033[36m%-20s\033[0m %s\n", $$1, $$2}'

info: ## env info
	@echo CFN_CHANGE_SET = $(CFN_CHANGE_SET)
	@echo GIT_FQDN = $(GIT_FQDN)
	@echo GIT_REF_NAME = $(GIT_REF_NAME)
	@echo GIT_REF_SHORT = $(GIT_REF_SHORT)
	@echo STACK_NAME = $(STACK_NAME)
	@echo PROFILE = $(PROFILE)

validate-stack: ## cfn validate template
	aws cloudformation validate-template --template-body file://./iam.template --profile ${PROFILE}

create-stack: ## cfn create stack
	aws cloudformation create-stack --stack-name ${STACK_NAME} --profile ${PROFILE} --capabilities CAPABILITY_NAMED_IAM --template-body file://./iam.template --parameters=ParameterKey=Password,ParameterValue=${PASSWORD}

delete-stack: ## cfn delete stack
	aws cloudformation delete-stack --stack-name ${STACK_NAME} --profile ${PROFILE}

update-stack:  ## update cfn template
	aws cloudformation create-change-set --change-set-name ${CFN_CHANGE_SET} --stack-name ${STACK_NAME} --capabilities CAPABILITY_NAMED_IAM --template-body file://./iam.template --parameters=ParameterKey=Password,ParameterValue=${PASSWORD} --profile ${PROFILE}
	aws cloudformation describe-change-set --change-set-name ${CFN_CHANGE_SET} --stack-name ${STACK_NAME} --profile ${PROFILE}
	sleep 10
	aws cloudformation execute-change-set --change-set-name ${CFN_CHANGE_SET} --stack-name ${STACK_NAME} --profile ${PROFILE}

output-stack: ## cfn output
	aws cloudformation describe-stacks --stack-name ${STACK_NAME} --profile ${PROFILE}
