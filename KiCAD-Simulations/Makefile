.PHONY: \
	all \
	help \
	list-sources \
	list-targets

# Hack to get the directory this makefile is in:
MKFILE_PATH := $(lastword $(MAKEFILE_LIST))
MKFILE_DIR := $(notdir $(patsubst %/,%,$(dir $(MKFILE_PATH))))
MKFILE_ABSDIR := $(abspath $(MKFILE_DIR))
MKFILE_DIRNAME:= $(shell basename $(MKFILE_ABSDIR))
SOURCES := $(shell cd $(MKFILE_DIR) ; ls *.cir )
TARGETS := $(shell echo $(SOURCES) | sed 's/\.cir//g' )
# Misc target info:
help_spacing  := 20

all: $(TARGETS)

help: ## Print this makefile help menu
	@printf "\n%s/Makefile:\n\n" "$(MKFILE_DIRNAME)"
	@echo "MAKEFILE TARGETS:"
	@grep '^[a-z_-]\{1,\}:.*##' $(MAKEFILE_LIST) \
		| sed 's/^\([a-z_\-]\{1,\}\): *\(.*[^ ]\) *## *\(.*\)/\1:\t\3 (\2)/g' \
		| sed 's/^\([a-z_\-]\{1,\}\): *## *\(.*\)/\1:\t\2/g' \
		| awk '{$$1 = sprintf("%-$(help_spacing)s", $$1)} 1' \
		| sed 's/^/  /'
	@printf "\n%s:\n" "SIMULATION TARGETS:"
	@for t in $(TARGETS); do \
		printf "%s: run %s.cir\n" $$t $$t \
		| awk '{$$1 = sprintf("%-$(help_spacing)s", $$1)} 1' \
		| sed 's/^/  /' \
		; done
	@echo ""

%: %.cir
	ngspice -b $<

list-sources: ## List all sources
	@echo "$(SOURCES)" | tr ' ' '\n' | sort

list-targets: ## List all targets
	@echo "$(TARGETS)" | tr ' ' '\n' | sort
