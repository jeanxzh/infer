# Copyright (c) 2015 - present Facebook, Inc.
# All rights reserved.
#
# This source code is licensed under the BSD style license found in the
# LICENSE file in the root directory of this source tree. An additional grant
# of patent rights can be found in the PATENTS file in the same directory.

ROOT_DIR = ../../..
include $(ROOT_DIR)/Makefile.config

INFER = INFER_ANALYZE_MODELS=1 $(INFER_BIN) \
          --buck --analyzer infer --multicore 1

JAVA_MODELS_SOURCES = $(shell find $(JAVA_MODELS_DIR)/src -name "*.java")

ANDROID_JAR = $(LIB_DIR)/java/android/android-19.jar
JACKSON_JAR = $(DEPENDENCIES_DIR)/java/jackson/jackson-2.2.3.jar
INFER_ANNOTATIONS_JAR = $(ANNOTATIONS_DIR)/annotations.jar

MODELS_OUT = models
INFER_REPORT = $(MODELS_OUT)/infer/report.json
MODELS_JAR = models.jar
DEPLOYED_MODELS_JAR = $(LIB_DIR)/java/models.jar

JAVA_SOURCES = $(shell find src/ -name "*.java")

all: $(MODELS_JAR)

clean:
	@rm -fr $(MODELS_OUT)
	@rm -f $(MODELS_JAR)
	@rm -f $(DEPLOYED_MODELS_JAR)

$(INFER_REPORT): $(ANDROID_JAR) $(JACKSON_JAR) $(JAVA_SOURCES) $(JAVA_DEPS) $(INFER_ANNOTATIONS_JAR)
	@rm -fr $(MODELS_OUT)
	mkdir -p $(MODELS_OUT)
	@rm -f $(DEPLOYED_MODELS_JAR)
	$(INFER) -- javac -bootclasspath $(ANDROID_JAR) -d $(MODELS_OUT) -classpath $(JACKSON_JAR):$(INFER_ANNOTATIONS_JAR) $(JAVA_SOURCES)

$(MODELS_JAR): $(INFER_REPORT)
	cd $(MODELS_OUT); jar cf ../$(MODELS_JAR) *

$(DEPLOYED_MODELS_JAR): $(MODELS_JAR)
	$(INSTALL) -m 644 -C $(MODELS_JAR) $(DEPLOYED_MODELS_JAR)

install: $(DEPLOYED_MODELS_JAR)

.PHONY: all clean install
