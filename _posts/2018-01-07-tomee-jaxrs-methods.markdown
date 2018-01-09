---
layout: post
title:  "How to send HTTP requests with unsupported methods in Tomee JAX-RS"
date:   2018-01-07 13:51:38 +0100
categories: java jaxrs tomee
---

Method java.net.HttpURLConnection.setRequestMethod intentionally doesn't allow to use other method than "good old ones".
This even doesn't include PATCH, specified in [RFC 5789][rfc5789].
And it will not be changed as stated [bug][here].


{% highlight java %}
import javax.ws.rs.client.*;
import org.apache.cxf.transport.http.*;


client = ClientBuilder....

// set property "use.httpurlconnection.method.reflection"
client.property(URLConnectionHTTPConduit.HTTPURL_CONNECTION_METHOD_REFLECTION, Boolean.TRUE.toString());
{% endhighlight %}

Internally, it's implemented by directly changing `method` field using Field.setAccessible().
You can check [URLConnectionHTTPConduit source code][source] and use this maven module:

{% highlight xml %}
<dependency>
    <groupId>org.apache.cxf</groupId>
    <artifactId>cxf-rt-transports-http</artifactId>
    <version>3.1.13</version>
    <scope>provided</scope>
</dependency>
{% endhighlight %}


[source]: https://github.com/apache/cxf/blob/master/rt/transports/http/src/main/java/org/apache/cxf/transport/http/URLConnectionHTTPConduit.java
[bug]: https://bugs.openjdk.java.net/browse/JDK-7016595
[rfc5789]: https://tools.ietf.org/html/rfc5789
