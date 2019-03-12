.PHONY: build clean

MICRODONUTS_JAR := target/api-1.0-SNAPSHOT.jar
SPECIALAGENT_JAR := opentracing-specialagent-0.9.1-SNAPSHOT.jar
LIGHTSTEP_TRACER_JAR := tracers/lightstep-tracer-jre-0.15.1.jar
JAEGER_TRACER_JAR := tracers/jaeger-client-0.33.2.jar

SERVICE_NAME := Microdonuts
LOGLEVEL := INFO

build:
	mvn package

run-with-lightstep: build $(SPECIALAGENT_JAR) $(LIGHTSTEP_TRACER_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dspecialagent.tracer=$(LIGHTSTEP_TRACER_JAR) \
		-Dspecialagent.log.level=$(LOGLEVEL) \
		-Dtracer.configurationFile=tracers/ls_tracer.properties \
		-Dls.componentName=$(SERVICE_NAME) \
		com.otsample.api.App

run-with-jaeger: build $(SPECIALAGENT_JAR) $(JAEGER_TRACER_JAR)
	java -cp $(MICRODONUTS_JAR) -javaagent:$(SPECIALAGENT_JAR) \
		-Dspecialagent.tracer=$(JAEGER_TRACER_JAR) \
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
