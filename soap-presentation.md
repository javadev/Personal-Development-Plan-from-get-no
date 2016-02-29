SOAP
====

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
