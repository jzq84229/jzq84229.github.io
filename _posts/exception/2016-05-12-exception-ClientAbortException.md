---
layout: post
title: org.apache.catalina.connector.ClientAbortException null
category: Exception
tags:
keywords:
description:
---

项目中遇到一个问题，前台向后台发送请求时，没有数据返回，后台报错：

    org.apache.catalina.connector.ClientAbortException: null
            at org.apache.catalina.connector.OutputBuffer.realWriteBytes(OutputBuffer.java:388) ~[catalina.jar:7.0.26]
            at org.apache.tomcat.util.buf.ByteChunk.flushBuffer(ByteChunk.java:462) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.tomcat.util.buf.ByteChunk.append(ByteChunk.java:366) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.catalina.connector.OutputBuffer.writeBytes(OutputBuffer.java:413) ~[catalina.jar:7.0.26]
            at org.apache.catalina.connector.OutputBuffer.write(OutputBuffer.java:401) ~[catalina.jar:7.0.26]
            at org.apache.catalina.connector.CoyoteOutputStream.write(CoyoteOutputStream.java:91) ~[catalina.jar:7.0.26]
            at com.fasterxml.jackson.core.json.UTF8JsonGenerator._flushBuffer(UTF8JsonGenerator.java:1853) ~[jackson-core-2.4.4.jar:2.4.4]
            at com.fasterxml.jackson.core.json.UTF8JsonGenerator.flush(UTF8JsonGenerator.java:1034) ~[jackson-core-2.4.4.jar:2.4.4]
            at com.fasterxml.jackson.databind.ObjectMapper.writeValue(ObjectMapper.java:1904) ~[jackson-databind-2.4.4.jar:2.4.4]
            at org.springframework.http.converter.json.AbstractJackson2HttpMessageConverter.writeInternal(AbstractJackson2HttpMessageConverter.java:231) ~[spring-web-4.1.3.RELEASE.j
    ar:4.1.3.RELEASE]
            at org.springframework.http.converter.AbstractHttpMessageConverter.write(AbstractHttpMessageConverter.java:208) ~[spring-web-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.mvc.method.annotation.AbstractMessageConverterMethodProcessor.writeWithMessageConverters(AbstractMessageConverterMethodProcessor.java:
    161) ~[spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.mvc.method.annotation.AbstractMessageConverterMethodProcessor.writeWithMessageConverters(AbstractMessageConverterMethodProcessor.java:
    101) ~[spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor.handleReturnValue(RequestResponseBodyMethodProcessor.java:202) ~[spring-webmv
    c-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.method.support.HandlerMethodReturnValueHandlerComposite.handleReturnValue(HandlerMethodReturnValueHandlerComposite.java:71) ~[spring-web-4.1.3
    .RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:126) ~[spring-webmvc-4.1.3.RELE
    ASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver.doResolveHandlerMethodException(ExceptionHandlerExceptionResolver.java:362) ~[
    spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.handler.AbstractHandlerMethodExceptionResolver.doResolveException(AbstractHandlerMethodExceptionResolver.java:60) [spring-webmvc-4.1.3
    .RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.handler.AbstractHandlerExceptionResolver.resolveException(AbstractHandlerExceptionResolver.java:138) [spring-webmvc-4.1.3.RELEASE.jar:
    4.1.3.RELEASE]
            at org.springframework.web.servlet.DispatcherServlet.processHandlerException(DispatcherServlet.java:1167) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.DispatcherServlet.processDispatchResult(DispatcherServlet.java:1004) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:955) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:877) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:966) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:868) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at javax.servlet.http.HttpServlet.service(HttpServlet.java:641) [servlet-api.jar:na]
            at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:842) [spring-webmvc-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at javax.servlet.http.HttpServlet.service(HttpServlet.java:722) [servlet-api.jar:na]
            at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:305) [catalina.jar:7.0.26]
            at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:210) [catalina.jar:7.0.26]
            at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:88) [spring-web-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-4.1.3.RELEASE.jar:4.1.3.RELEASE]
            at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:243) [catalina.jar:7.0.26]
            at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:210) [catalina.jar:7.0.26]
            at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:224) [catalina.jar:7.0.26]
            at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:169) [catalina.jar:7.0.26]
            at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:472) [catalina.jar:7.0.26]
            at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:168) [catalina.jar:7.0.26]
            at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:98) [catalina.jar:7.0.26]
            at org.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:927) [catalina.jar:7.0.26]
            at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:118) [catalina.jar:7.0.26]
            at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:407) [catalina.jar:7.0.26]
            at org.apache.coyote.http11.AbstractHttp11Processor.process(AbstractHttp11Processor.java:987) [tomcat-coyote.jar:7.0.26]
            at org.apache.coyote.AbstractProtocol$AbstractConnectionHandler.process(AbstractProtocol.java:579) [tomcat-coyote.jar:7.0.26]
            at org.apache.tomcat.util.net.JIoEndpoint$SocketProcessor.run(JIoEndpoint.java:307) [tomcat-coyote.jar:7.0.26]
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145) [na:1.7.0_79]
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615) [na:1.7.0_79]
            at java.lang.Thread.run(Thread.java:745) [na:1.7.0_79]
    Caused by: java.net.SocketException: 断开的管道
            at java.net.SocketOutputStream.socketWrite0(Native Method) ~[na:1.7.0_79]
            at java.net.SocketOutputStream.socketWrite(SocketOutputStream.java:113) ~[na:1.7.0_79]
            at java.net.SocketOutputStream.write(SocketOutputStream.java:159) ~[na:1.7.0_79]
            at org.apache.coyote.http11.InternalOutputBuffer.realWriteBytes(InternalOutputBuffer.java:215) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.tomcat.util.buf.ByteChunk.flushBuffer(ByteChunk.java:462) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.tomcat.util.buf.ByteChunk.append(ByteChunk.java:366) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.coyote.http11.InternalOutputBuffer$OutputStreamOutputBuffer.doWrite(InternalOutputBuffer.java:240) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.coyote.http11.filters.IdentityOutputFilter.doWrite(IdentityOutputFilter.java:93) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.coyote.http11.AbstractOutputBuffer.doWrite(AbstractOutputBuffer.java:192) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.coyote.Response.doWrite(Response.java:533) ~[tomcat-coyote.jar:7.0.26]
            at org.apache.catalina.connector.OutputBuffer.realWriteBytes(OutputBuffer.java:383) ~[catalina.jar:7.0.26]
            ... 47 common frames omitted


分析：
    socketException 管道断开；
    检查发现，这个接口调用时间很长，有12-20秒，导致客户端的链接请求超时，断开链接，返回null。  
    解决方法：修改客户端请求超时时间，设置为30秒，问题解决。  
    后期优化：分析服务端接口超长耗时原因，修改接口实现提高执行效率。  
