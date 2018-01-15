---
layout: post
title:  "How to send HTTP requests with unsupported methods in Tomee JAX-RS"
date:   2018-01-07 13:51:38 +0100
categories: java
---

When working with REST services you can easily come across a service which utilizes other method than those specified in [HTTP/1.1 RFC][rfc2616].
Usually it will be PATCH method which is specified in [RFC 5789][rfc5789].
If you use CXF framework client in Tomee, you will probably get `Invalid HTTP method` exception.

Problem is that CXF uses standard java.net HTTP client, which has fixed set of allowed methods.
This list doesn't even include above mentioned PATCH and it's not going to be changed as is stated in bug report [here][bug].

Fortunately, CXF has quick and dirty fix to address this issue:

{% highlight java %}
import javax.ws.rs.client.*;
import org.apache.cxf.transport.http.*;


Client client = ClientBuilder....

// set property "use.httpurlconnection.method.reflection"
client.property(URLConnectionHTTPConduit.HTTPURL_CONNECTION_METHOD_REFLECTION, Boolean.TRUE.toString());
{% endhighlight %}

Internally, it's implemented by directly changing `method` field in `java.net.HttpURLConnection` using `Field.setAccessible()`.
You can check [URLConnectionHTTPConduit source code][source].
This code is delivered as maven module:

{% highlight xml %}
<dependency>
    <groupId>org.apache.cxf</groupId>
    <artifactId>cxf-rt-transports-http</artifactId>
    <version>3.1.13</version>
    <scope>provided</scope>
</dependency>
{% endhighlight %}

Note: the property can also be set globally using `System.setProperty()` method or with command line parameter `-Duse.httpurlconnection.method.reflection=true`.

[source]: https://github.com/apache/cxf/blob/master/rt/transports/http/src/main/java/org/apache/cxf/transport/http/URLConnectionHTTPConduit.java
[bug]: https://bugs.openjdk.java.net/browse/JDK-7016595
[rfc5789]: https://tools.ietf.org/html/rfc5789
[rfc2616]: https://tools.ietf.org/html/rfc2616
