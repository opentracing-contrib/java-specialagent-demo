# MicroDonuts: An OpenTracing SpecialAgent Demo

Welcome to MicroDonuts! This is a [SpecialAgent](https://github.com/opentracing-contrib/java-specialagent)
demo application, written in Java, based on the original [OpenTracing walkthrough](https://github.com/opentracing-contrib/java-opentracing-walkthrough).

OpenTracing is a vendor-neutral, open standard for distributed tracing. To
learn more, check out [opentracing.io](http://opentracing.io), and try the
demo application below!

## Step 0: Setup MicroDonuts

### Getting it
Clone this repository and build the jar file (for this, Maven must be
installed):

```
git clone git@github.com:opentracing-contrib/java-opentracing-specialagent-demo.git
cd java-opentracing-specialagent-demo/microdonuts
mvn package
```

### Running

MicroDonuts has two server components, `API` and `Kitchen`, which
communicate each other over HTTP - they are, however, part of
the same process:

```
cd java-opentracing-specialagent-demo/microdonuts
mvn package exec:exec
```

In your web broswer, navigate to http://127.0.0.1:10001 and order yourself some
µ-donuts.

### Provide a TracerFactory jar.

A jar containing a [TracerFactory](https://github.com/opentracing-contrib/java-tracerresolver) providing a `Tracer` is required,
such as `jaeger.jar` or `lightstep.jar`.

#### Jaeger

To run Jaeger locally (via Docker):

```bash
$ docker run -d -p 5775:5775/udp -p 16686:16686 jaegertracing/all-in-one:latest
```

Then add the following to `microdonuts/tracer_config.properties`:

```properties
JAEGER_SERVICE_NAME=MicroDonuts
```

Note that the all-in-one docker image presents the Jaeger UI at [localhost:16686](http://localhost:16686/).

#### LightStep

If you have access to [LightStep](https://app.lightstep.com]), you will need your access token. Add the following to `microdonuts/tracer_config.properties`:

```properties
ls.collectorProtocol=https
ls.collectorHost=collector.lightstep.com
ls.collectorPort=443
ls.componentName=MicroDonuts
ls.accessToken=XXXXXXXXXXXXXXX  // TODO: replace with your token
```

### Check it out in your Tracer

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
