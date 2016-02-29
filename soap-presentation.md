Agenda

Calculator soap service

 - SOAP description
 - SOAP server
 - SOAP client

SOAP description
================

SOAP, originally an acronym for Simple Object Access Protocol, is a protocol specification for exchanging structured
information in the implementation of web services in computer networks. It uses XML Information Set for its message format,
and relies on application layer protocols, most notably Hypertext Transfer Protocol (HTTP) or Simple Mail Transfer Protocol
(SMTP), for message negotiation and transmission.

SOAP Building Blocks
====================

|Element |Description|Required|
|--------|---|---|
|Envelope|Identifies the XML document as a SOAP message.|Yes|
|Header  |Contains header information.|No|
|Body    |Contains call, and response information.|Yes|
|Fault   |Provides information about errors that occurred while processing the message.|No|

Example message
===============

```xml
POST /InStock HTTP/1.1
Host: www.example.org
Content-Type: application/soap+xml; charset=utf-8
Content-Length: 299
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"

<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
  </soap:Header>
  <soap:Body>
    <m:GetStockPrice xmlns:m="http://www.example.org/stock/Surya">
      <m:StockName>IBM</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>
```

Technical critique
==================

Advantages
----------

* SOAP's neutrality characteristic explicitly makes it suitable for use with any transport protocol. Implementations often use HTTP as a transport protocol, but obviously other popular transport protocols can be used. For example, SOAP can also be used over SMTP, JMS and Message Queues.
* SOAP, when combined with HTTP post/response exchanges, tunnels easily through existing firewalls and proxies, and consequently doesn't require modifying the widespread computing and communication infrastructures that exist for processing HTTP post/response exchanges.

Disadvantages
-------------

* When using standard implementations and the default SOAP/HTTP binding, the XML infoset is serialized as XML. Because of the verbose XML format, SOAP can be considerably slower than competing middleware technologies such as CORBA or ICE.[14] This may not be an issue when only small messages are sent.[15] To improve performance for the special case of XML with embedded binary objects, the Message Transmission Optimization Mechanism was introduced.
* When relying on HTTP as a transport protocol and not using WS-Addressing or an ESB, the roles of the interacting parties are fixed. Only one party (the client) can use the services of the other.
* The verbosity of the protocol led to the domination in the field by services using the HTTP protocol more directly.

SOAP Versions
=============

|SOAP 1.1||
|--------|---|
|Namespace Name|http://schemas.xmlsoap.org/soap/envelope/|
|Spec Location|http://www.w3.org/TR/SOAP/|
|SOAP 1.2||
|Namespace Name|http://www.w3.org/2002/12/soap-envelope|
|Spec Location|http://www.w3.org/TR/soap12-part0/ (Primer)|
|http://www.w3.org/TR/soap12-part1/||
|http://www.w3.org/TR/soap12-part2/||

SOAP server, Spring Web Service server sample application
=========================================================

depenency module in pom.xml:

```
    <dependency>
      <groupId>org.springframework.ws</groupId>
      <artifactId>spring-ws-core</artifactId>
      <version>2.1.0.RELEASE</version>
    </dependency>
```

Jetty plugin:
=============

```
  <plugin>
      <groupId>org.mortbay.jetty</groupId>
      <artifactId>jetty-maven-plugin</artifactId>
      <version>${jetty.version}</version>
      <configuration>
          <webAppConfig>
              <contextPath>/LiveRestaurant</contextPath>
          </webAppConfig>
          <connectors>
              <connector implementation="org.eclipse.jetty.server.nio.SelectChannelConnector">
                  <port>8080</port>
                  <maxIdleTime>3600000</maxIdleTime>
              </connector>
          </connectors>
      </configuration>
  </plugin>
```

Add endpont class
=================

```java
package com.live.order.service.endpoint;

import javax.xml.bind.JAXBElement;
import javax.xml.namespace.QName;

import org.springframework.ws.server.endpoint.annotation.Endpoint;
import org.springframework.ws.server.endpoint.annotation.PayloadRoot;

import com.liverestaurant.orderservice.schema.CancelOrderRequest;
import com.liverestaurant.orderservice.schema.CancelOrderResponse;
import com.liverestaurant.orderservice.schema.ObjectFactory;
import com.liverestaurant.orderservice.schema.PlaceOrderRequest;
import com.liverestaurant.orderservice.schema.PlaceOrderResponse;
import com.live.order.service.OrderService;

/**
* <pre>
* This is the endpoint for the {@link OrderService}.
* Requests are simply delegated to the {@link OrderService} for processing.
* Two operations are mapped, using annotation, as specified in the link,
* <a href="http://static.springsource.org/spring-ws/sites/1.5/reference/html/server.html#server-at-endpoint"
* >http://static.springsource.org/spring-ws/sites/1.5/reference/html/server.html#server-at-endpoint</a
* ><ul>
*     <li>placeOrderRequest</li>
*     <li>cancelOrderRequest</li>
* </ul>
* </pre>
*/
@Endpoint
public class OrderServicePayloadRootAnnotationEndPoint {

    private final OrderService orderService;
    private final ObjectFactory JAXB_OBJECT_FACTORY = new ObjectFactory();

    public OrderServicePayloadRootAnnotationEndPoint(OrderService orderService) {
        this.orderService = orderService;
    }

    @PayloadRoot(localPart = "placeOrderRequest", namespace = "http://www.liverestaurant.com/OrderService/schema")
    public JAXBElement<PlaceOrderResponse> getOrder(
            PlaceOrderRequest placeOrderRequest) {
        PlaceOrderResponse response = JAXB_OBJECT_FACTORY
                .createPlaceOrderResponse();
        response.setRefNumber(orderService.placeOrder(placeOrderRequest
                .getOrder()));

        return new JAXBElement<PlaceOrderResponse>(new QName(
                "http://www.liverestaurant.com/OrderService/schema",
                "placeOrderResponse"), PlaceOrderResponse.class, response);
    }

    @PayloadRoot(localPart = "cancelOrderRequest", namespace = "http://www.liverestaurant.com/OrderService/schema")
    public JAXBElement<CancelOrderResponse> cancelOrder(
            CancelOrderRequest cancelOrderRequest) {
        CancelOrderResponse response = JAXB_OBJECT_FACTORY
                .createCancelOrderResponse();
        response.setCancelled(orderService.cancelOrder(cancelOrderRequest
                .getRefNumber()));
        return new JAXBElement<CancelOrderResponse>(new QName(
                "http://www.liverestaurant.com/OrderService/schema",
                "cancelOrderResponse"), CancelOrderResponse.class, response);
    }

}
```

