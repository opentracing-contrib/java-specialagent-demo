.PHONY: build clean

MICRODONUTS_JAR := target/api-1.0-SNAPSHOT.jar
SPECIALAGENT_JAR := opentracing-specialagent-1.0.0.jar

SERVICE_NAME := Microdonuts
LOGLEVEL := INFO

build:
	mvn package

run-with-lightstep: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dspecialagent.tracer=lightstep \
		-Dspecialagent.log.level=$(LOGLEVEL) \
		-Dtracer.configurationFile=ls_tracer.properties \
		-Dls.componentName=$(SERVICE_NAME) \
		com.otsample.api.App

run-with-jaeger: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dspecialagent.tracer=jaeger \
		-Dspecialagent.log.level=$(LOGLEVEL) \
		-DJAEGER_SERVICE_NAME=$(SERVICE_NAME) \
		com.otsample.api.App

run-with-tracer-jar: build $(SPECIALAGENT_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dspecialagent.tracer=$(TRACER_JAR) \
		-Dspecialagent.log.level=$(LOGLEVEL) \
		com.otsample.api.App

run-no-agent: build
	java -cp $(MICRODONUTS_JAR) com.otsample.api.App

clean:
	mvn clean
