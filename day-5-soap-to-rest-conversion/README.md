# Week 8 – Day 5: SOAP to REST Conversion

**Theme:** XML & SOAP Services
**Focus:** Converting a SOAP service into a REST API

---

## Overview

Today I explored how SOAP services can be wrapped or converted into REST APIs. Many modern applications prefer REST because it is lightweight, easier to consume, and typically uses JSON instead of XML.

However, many enterprise systems still expose SOAP APIs. Instead of rewriting those systems, developers often build a **REST wrapper** that communicates with the SOAP service internally.

In this session I:

* Learned why SOAP-to-REST conversion is common
* Built a REST wrapper for a SOAP service
* Transformed XML responses into JSON

---

## Why Convert SOAP to REST?

SOAP is powerful but heavy. REST is often preferred for modern web and mobile applications.

### SOAP Characteristics

* XML only
* Strict contracts (WSDL)
* Larger payload sizes
* More complex clients

### REST Characteristics

* JSON support
* Lightweight
* Simpler endpoints
* Easier frontend integration

Because of this, many teams expose REST APIs that internally call SOAP services.

---

## Architecture

Typical SOAP-to-REST architecture:

```
Client → REST API → SOAP Client → SOAP Service
```

Flow:

1. REST client sends HTTP request
2. REST server calls SOAP service
3. SOAP response returns XML
4. REST server converts XML to JSON
5. JSON response returned to client

---

## Example REST Wrapper (Java Spring Boot)

Example controller:

```java
@RestController
@RequestMapping("/api/calculator")
public class CalculatorController {

    @GetMapping("/add")
    public Map<String, Integer> add(
        @RequestParam int a,
        @RequestParam int b
    ) {
        Calculator service = new Calculator();
        CalculatorSoap soap = service.getCalculatorSoap();

        int result = soap.add(a, b);

        Map<String, Integer> response = new HashMap<>();
        response.put("result", result);

        return response;
    }
}
```

---

## REST Request Example

```
GET /api/calculator/add?a=5&b=3
```

Response (JSON):

```json
{
  "result": 8
}
```

---

## What Happens Internally

When the REST endpoint is called:

1. Client sends HTTP request
2. Spring controller receives request
3. Controller calls SOAP client
4. SOAP service returns XML response
5. Application converts response to JSON
6. JSON is returned to the client

This allows modern applications to interact with legacy SOAP systems seamlessly.

---

## Key Engineering Concepts I Learned

* How REST can act as a gateway to SOAP services
* How to integrate SOAP clients inside REST APIs
* Transforming XML responses into JSON
* Designing integration layers for legacy systems

---

## Production Considerations

If this were a production system I would also implement:

* Caching for SOAP responses
* Error handling and retry logic
* Authentication between services
* Rate limiting
* API documentation (OpenAPI)

These improvements help ensure reliability and scalability.

---

## What I Learned

Through this exercise I learned that SOAP services do not need to be replaced immediately when building modern systems. Instead, they can be integrated through a REST layer that exposes simpler endpoints while still leveraging existing SOAP infrastructure.

