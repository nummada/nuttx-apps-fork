############################################################################
# apps/gpsutils/minmea/Makefile
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.  The
# ASF licenses this file to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance with the
# License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
# License for the specific language governing permissions and limitations
# under the License.
#
############################################################################

include $(APPDIR)/Make.defs

$(info ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~)
$(info ~~~~coaputils/coap/Makefile~~~~)
$(info ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~)

COAP_URL ?= "https://github.com/obgm/libcoap/archive"
COAP_VERSION ?= afbac63b283bcd96665bff7107f2b83edce4f228

COAP_UNPACKNAME = libcoap

$(COAP_UNPACKNAME):
	@echo "Downloading: $(COAP_UNPACKNAME)"
	$(Q) curl -O -L $(COAP_URL)/$(COAP_VERSION).zip
	$(Q) mkdir $(COAP_UNPACKNAME)
	$(Q) unzip -q -o $(COAP_VERSION).zip -d $(COAP_UNPACKNAME)
	$(Q) mv $(COAP_UNPACKNAME)/libcoap-$(COAP_VERSION)/* $(COAP_UNPACKNAME)
	$(Q) rm -r $(COAP_UNPACKNAME)/libcoap-$(COAP_VERSION)
	$(call DELFILE, $(COAP_VERSION).zip)

# Files

# Build COAP using coap specific build system

COAP_CURRENT_DIR := $(shell pwd)
COAP_UTILS_DIR := $(COAP_CURRENT_DIR)/..
COAP_UTILS_INCLUDE_DIR := $(COAP_CURRENT_DIR)/include
COAP_UTILS_LIB_DIR := $(COAP_CURRENT_DIR)/lib

COAP_DELDIR := $(COAP_UNPACKNAME)
COAP_DELDIR += install_coap
COAP_DELDIR += include
COAP_DELDIR += lib


build-$(COAP_UNPACKNAME): $(COAP_UNPACKNAME)

	@(echo ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~)
	@(echo ~~~~~Building coap library~~~~~)
	@(echo ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~)

	@(echo "Building coap library")
	mkdir -p $(COAP_CURRENT_DIR)/install_coap
	./$(COAP_UNPACKNAME)/autogen.sh

# Each command is exectued in a separate shell, so in oder to `cd` and execute
# commands, we need to chanin the commands to the `cd` command by using `&&` bash operator
# TODO 2: `make` this lib for the correct architecture. As of now it is being build for
# idk, I guess x86_64 (local system architecture)
	cd $(COAP_UNPACKNAME) && \
		./configure \
			-q \
			--disable-documentation \
			--disable-shared \
			--disable-dtls \
			--prefix=$(COAP_CURRENT_DIR)/install_coap \
			--includedir=$(COAP_UTILS_INCLUDE_DIR) \
			--libdir=$(COAP_UTILS_LIB_DIR) && \
		CC=$(CC) make -s install
	
	@(echo ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~)
	@(echo ~~Building coap library DONE~~~)
	@(echo ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~)

# TODO 1: When is `LDLIBS` used? Because I need to link `libcoap-3.a` into the final executable
# LDLIBS += -l$(COAP_UTILS_LIB_DIR)/libcoap-3.a

clean::
	$(call DELFILE, $(OBJS))

# Download and unpack tarball if no git repo found
ifeq ($(wildcard $(COAP_UNPACKNAME)/.git),)

# Create context meaning downloading the source code of the library
# Also as context, we need to build the library
context:: build-$(COAP_UNPACKNAME) # also build coap

distclean::
	$(call DELDIR, $(COAP_DELDIR))
	$(call DELFILE, ar-lib)
endif

include $(APPDIR)/Application.mk
