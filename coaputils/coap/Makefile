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

COAP_URL ?= "https://github.com/obgm/libcoap/archive"
COAP_VERSION ?= afbac63b283bcd96665bff7107f2b83edce4f228

COAP_UNPACKNAME = libcoap

$(COAP_UNPACKNAME):
	@echo "Downloading: $(COAP_UNPACKNAME)"
	$(Q) curl -O -L $(COAP_URL)/$(COAP_VERSION).zip
	$(Q) mkdir $(COAP_UNPACKNAME)
	$(Q) unzip -o $(COAP_VERSION).zip -d $(COAP_UNPACKNAME)
	$(Q) mv $(COAP_UNPACKNAME)/libcoap-$(COAP_VERSION)/* $(COAP_UNPACKNAME)
	$(Q) rm -r $(COAP_UNPACKNAME)/libcoap-$(COAP_VERSION)
	$(call DELFILE, $(COAP_VERSION).zip)

# Files

# Build COAP using coap specific build system

$(info Salut bossean!!!!)

CSRCS	= minmea/minmea.c
CFLAGS += -std=c99

build-$(COAP_UNPACKNAME): $(COAP_UNPACKNAME)
	@(echo "Building coap library")
	mkdir $(COAP_UNPACKNAME)/install_coap
	./$(COAP_UNPACKNAME)/autogen.sh
	./$(COAP_UNPACKNAME)/configure \
		--disable-documentation \
		--disable-shared \
		--disable-dtls \
		--prefix=`pwd`/install_coap

clean::
	$(call DELFILE, $(OBJS))

# Download and unpack tarball if no git repo found
ifeq ($(wildcard $(COAP_UNPACKNAME)/.git),)

# Create context meaning downloading the source code of the library
# Also as context, we need to build the library
context:: build-$(COAP_UNPACKNAME) # also build coap

distclean::
	$(call DELDIR, $(COAP_UNPACKNAME))
endif

include $(APPDIR)/Application.mk
