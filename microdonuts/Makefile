.PHONY: build clean

MICRODONUTS_JAR := $(shell find target -name 'api-*.jar')
SPECIALAGENT_JAR := $(shell find . -name 'opentracing-specialagent-*.jar')

SERVICE_NAME := Microdonuts
LOGLEVEL := INFO

build:
	mvn package

run-with-mock: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dsa.exporter=mock \
		-Dsa.log.level=$(LOGLEVEL) \
		com.otsample.api.App

run-with-lightstep: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dsa.exporter=lightstep \
		-Dsa.log.level=$(LOGLEVEL) \
		-Dsa.config=ls_tracer.properties \
		-Dls.componentName=$(SERVICE_NAME) \
		com.otsample.api.App

run-with-jaeger: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dsa.exporter=jaeger \
		-Dsa.log.level=$(LOGLEVEL) \
		-DJAEGER_SERVICE_NAME=$(SERVICE_NAME) \
		com.otsample.api.App

run-with-tracer-jar: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dsa.exporter=$(TRACER_JAR) \
		-Dsa.log.level=$(LOGLEVEL) \
		com.otsample.api.App

run-no-agent: build
	java -cp $(MICRODONUTS_JAR) \
		com.otsample.api.App

clean:
	mvn clean
