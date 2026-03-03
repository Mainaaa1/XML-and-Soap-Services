# Week 8 — Day 2

# SOAP Basics & WSDL

Objective: Understand the SOAP protocol, message structure, and how WSDL defines service contracts.

---

# What is SOAP?

SOAP (Simple Object Access Protocol) is a protocol for exchanging structured information in web services using XML.

Key Characteristics:

* XML-based messaging
* Protocol independent (HTTP, SMTP, etc.)
* Strict contract definition
* Built-in error handling (Faults)
* Enterprise-focused (banking, telecom, government)

SOAP is highly structured and contract-driven compared to REST.

---

# SOAP Message Structure

A SOAP message is an XML document with 4 main parts:

```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">

  <soap:Header>
    <!-- Optional metadata -->
  </soap:Header>

  <soap:Body>
    <getUserRequest>
      <id>10</id>
    </getUserRequest>
  </soap:Body>

</soap:Envelope>
```

Components:

* Envelope → Root element
* Header → Authentication, tokens, metadata (optional)
* Body → Actual request/response data
* Fault → Error handling structure

---

# SOAP Response Example

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <getUserResponse>
      <name>Ian</name>
      <email>ian@example.com</email>
    </getUserResponse>
  </soap:Body>
</soap:Envelope>
```

---

# SOAP Fault Example

```xml
<soap:Fault>
  <faultcode>soap:Client</faultcode>
  <faultstring>Invalid User ID</faultstring>
</soap:Fault>
```

SOAP provides structured, predictable error responses.

---

# What is WSDL?

WSDL (Web Services Description Language) is an XML document that describes:

* Service operations
* Input and output messages
* Data types (via XSD)
* Communication protocol
* Service endpoint (URL)

WSDL acts as a contract between client and server.

---

# WSDL Core Sections

1. Types → Data structures (XSD)
2. Message → Request/response format
3. PortType → Available operations
4. Binding → Protocol and format (usually SOAP over HTTP)
5. Service → Endpoint location

---

# Simplified WSDL Example

```xml
<definitions name="UserService"
  xmlns="http://schemas.xmlsoap.org/wsdl/"
  xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/">

  <message name="GetUserRequest">
    <part name="id" type="xsd:int"/>
  </message>

  <message name="GetUserResponse">
    <part name="name" type="xsd:string"/>
  </message>

  <portType name="UserPortType">
    <operation name="GetUser">
      <input message="tns:GetUserRequest"/>
      <output message="tns:GetUserResponse"/>
    </operation>
  </portType>

</definitions>
```

---

# SOAP vs REST (Architectural Comparison)

SOAP:

* Strict contract (WSDL)
* XML only
* Built-in security standards (WS-Security)
* Heavy but structured

REST:

* No strict contract by default
* JSON commonly used
* Lightweight
* Faster for web/mobile APIs

SOAP is preferred in enterprise systems requiring strict validation and transactional guarantees.

---

# Real-World Use Cases

* Banking systems
* Payment gateways
* Government integrations
* Enterprise ERP systems

Many legacy enterprise systems still rely heavily on SOAP.

---

# What I Learned

* SOAP is protocol-based and XML-driven
* Messages follow a strict envelope structure
* WSDL defines the service contract
* SOAP provides standardized fault handling
* Contract-first design ensures interoperability
