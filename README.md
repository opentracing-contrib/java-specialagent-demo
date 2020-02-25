# MicroDonuts: An OpenTracing SpecialAgent Demo

[![Build Status](https://travis-ci.org/opentracing-contrib/java-specialagent-demo.png)](https://travis-ci.org/opentracing-contrib/java-specialagent-demo)

Welcome to MicroDonuts! This is a [SpecialAgent](https://github.com/opentracing-contrib/java-specialagent)
demo application, written in Java, based on the original [OpenTracing walkthrough](https://github.com/opentracing-contrib/java-opentracing-walkthrough).

OpenTracing is a vendor-neutral, open standard for distributed tracing. To
learn more, check out [opentracing.io](http://opentracing.io), and try the
demo application below!

## Step 0: Setup MicroDonuts

### Getting it
Clone this repository and build the jar file (for this, Maven must be
installed):

```bash
git clone git@github.com:opentracing-contrib/java-specialagent-demo.git
cd java-specialagent-demo/microdonuts
make
```

### Running

MicroDonuts has two server components, `API` and `Kitchen`, which
communicate each other over HTTP - they are, however, part of
the same process:

```bash
cd java-specialagent-demo/microdonuts
make run-no-agent
```

In your web broswer, navigate to http://127.0.0.1:10001 and order yourself some
µ-donuts.

### Run with the SpecialAgent and a Tracer

First, please download the latest [SpecialAgent JAR](https://repository.sonatype.org/service/local/artifact/maven/redirect?r=central-proxy&g=io.opentracing.contrib.specialagent&a=opentracing-specialagent&v=LATEST)
and move it to the microdonuts directory:

```bash
mv opentracing-specialagent-*.jar microdonuts/
```

#### Jaeger

To run Jaeger locally ([via Docker](https://www.jaegertracing.io/docs/latest/getting-started/#all-in-one)):

```bash
$ docker run -d --name jaeger \
    -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 \
    -p 5775:5775/udp \
    -p 6831:6831/udp \
    -p 6832:6832/udp \
    -p 5778:5778 \
    -p 16686:16686 \
    -p 14268:14268 \
    -p 14250:14250 \
    -p 9411:9411 \
    jaegertracing/all-in-one::latest
```

To run MicroDonuts with Jaegger, configuration has to be specified through
[environment variables](https://github.com/jaegertracing/jaeger-client-java/blob/master/jaeger-core/README.md):

```bash
cd microdonuts
make run-with-jaeger
```

Note that the all-in-one docker image presents the Jaeger UI at [localhost:16686](http://localhost:16686/).

#### LightStep

If you have access to [LightStep](https://go.lightstep.com/tracing.html), you will need your access token. Add the following to `ls_tracer.properties`:

```properties
ls.accessToken=XXXXXXXXXXXXXXX  // TODO: replace with your token
```

To run MicroDonuts with LightStep:

```bash
cd microdonuts
make run-with-lightstep
```

### Provide your own Tracer

A provided jar containing any `TracerFactory` provider can be used to run MicroDonuts too:

```bash
cd microdonuts
env TRACER_JAR=MyOwnTracer.jar make run-with-tracer-jar
```

Now that we're all hooked up, try ordering some donuts in the browser. You
should see the traces appear in your tracer.

Search for traces starting belonging to the `MicroDonuts` component to see the
patterns of requests that occur when you click the order button.

## Thanks for playing, and welcome to OpenTracing!

Thanks for joining us in this walkthrough! Hope you enjoyed it. If you did, let
us know, and consider spreading the love!

A great way to get the feel for OpenTracing is to try your hand at
instrumenting the OSS servers, frameworks, and client libraries that we all
share. If you make one, consider adding it to the growing ecosystem at
http://github.com/opentracing-contrib. If you maintain a library yourself,
plase consider adding built-in OT support.

We also need walkthroughs for languages other than Golang. Feel free to reuse
the client, protobufs, and other assets from here if you'd like to make one.

For a more detailed explanation of OSS Instrumentation, check out the Turnkey
Tracing proposal at http://bit.ly/turnkey-tracing.

_Aloha!_