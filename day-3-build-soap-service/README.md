# Week 8 — Day 3

# Build a SOAP Service

Objective: Implement a basic SOAP web service and expose operations using a WSDL contract.

---

# Service Architecture Overview

In SOAP architecture:

Client → Sends XML request → SOAP Service → Processes → Returns XML response

Core Components:

* Service class (business logic)
* WSDL file (contract definition)
* SOAP server
* SOAP client

Today focuses on building the server.

---

# Step 1 — Define the Service Logic (PHP Example)

```php
<?php
class UserService {

    public function getUser($id) {
        $users = [
            1 => ["name" => "Ian", "email" => "ian@example.com"],
            2 => ["name" => "Alice", "email" => "alice@example.com"]
        ];

        if (!isset($users[$id])) {
            throw new SoapFault("Client", "User not found");
        }

        return $users[$id];
    }
}
?>
```

This class contains the business logic exposed through SOAP.

---

# Step 2 — Create SOAP Server

```php
<?php
$options = [
    'uri' => 'http://localhost/soap-server'
];

$server = new SoapServer(null, $options);
$server->setClass('UserService');
$server->handle();
?>
```

This creates a non-WSDL SOAP server (for learning purposes).

---

# WSDL-Based SOAP Server (Recommended)

```php
<?php
$server = new SoapServer("userservice.wsdl");
$server->setClass('UserService');
$server->handle();
?>
```

WSDL ensures strict contract enforcement.

---

# Sample WSDL Structure (Simplified)

```xml
<definitions name="UserService"
 xmlns="http://schemas.xmlsoap.org/wsdl/"
 xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/"
 xmlns:tns="http://localhost/userservice"
 targetNamespace="http://localhost/userservice">

 <message name="getUserRequest">
   <part name="id" type="xsd:int"/>
 </message>

 <message name="getUserResponse">
   <part name="name" type="xsd:string"/>
   <part name="email" type="xsd:string"/>
 </message>

 <portType name="UserPortType">
   <operation name="getUser">
     <input message="tns:getUserRequest"/>
     <output message="tns:getUserResponse"/>
   </operation>
 </portType>

</definitions>
```

---

# Step 3 — Testing with SOAP Client

```php
<?php
$client = new SoapClient("http://localhost/userservice.wsdl");
$response = $client->getUser(1);
print_r($response);
?>
```

Expected Output:

```
Array
(
    [name] => Ian
    [email] => ian@example.com
)
```

---

# SOAP Request (Raw XML Example)

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <getUser>
      <id>1</id>
    </getUser>
  </soap:Body>
</soap:Envelope>
```

---

# SOAP Response (Raw XML)

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

# Key Engineering Concepts Learned

* Contract-first development with WSDL
* Strict XML-based communication
* Structured fault handling
* Server-client separation
* Enterprise interoperability principles

---

# Production Considerations

In real systems:

* Implement authentication (WS-Security)
* Use HTTPS
* Validate requests against XSD
* Implement logging and monitoring
* Handle versioning carefully

SOAP services are common in banking and enterprise integrations where strict contracts and security standards are required.

---

# What I Learned

* How to expose PHP methods via SOAP
* How WSDL defines operations
* How SOAP enforces structure and validation
* Difference between WSDL and non-WSDL servers
