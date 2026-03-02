# Week 8 — Day 1

# XML Schema Design (XSD)

Objective: Understand XML structure and learn how to formally define and validate XML documents using XML Schema (XSD).

---

# What is XML?

XML (Extensible Markup Language) is a markup language used to store and transport structured data.

Example XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<book>
    <title>Learning SOAP</title>
    <author>Ian Muchelule</author>
    <year>2026</year>
</book>
```

XML focuses on data structure rather than presentation.

---

# XML Rules

* Must have a single root element
* Tags must close properly
* Tags are case-sensitive
* Attribute values must be quoted
* Proper nesting is required

Invalid Example:

```xml
<book>
    <title>XML Basics
</book>
```

---

# What is XSD?

XSD (XML Schema Definition) defines:

* Allowed elements
* Data types
* Required vs optional fields
* Structure and hierarchy
* Attribute constraints

It ensures XML documents follow strict rules.

---

# Basic XSD Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

  <xs:element name="book">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="title" type="xs:string"/>
        <xs:element name="author" type="xs:string"/>
        <xs:element name="year" type="xs:integer"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>

</xs:schema>
```

---

# Data Types in XSD

Common built-in types:

* xs:string
* xs:integer
* xs:decimal
* xs:boolean
* xs:date
* xs:dateTime

Example with constraints:

```xml
<xs:element name="price">
  <xs:simpleType>
    <xs:restriction base="xs:decimal">
      <xs:minInclusive value="0"/>
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

---

# Complex Types

Used when elements contain other elements or attributes.

```xml
<xs:complexType name="UserType">
  <xs:sequence>
    <xs:element name="name" type="xs:string"/>
    <xs:element name="email" type="xs:string"/>
  </xs:sequence>
  <xs:attribute name="id" type="xs:int" use="required"/>
</xs:complexType>
```

---

# XML with Schema Reference

```xml
<?xml version="1.0"?>
<book xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:noNamespaceSchemaLocation="book.xsd">
    <title>Learning SOAP</title>
    <author>Ian</author>
    <year>2026</year>
</book>
```

---

# Why XML Schema Matters for SOAP

SOAP services rely heavily on strict XML structure. XSD ensures:

* Strong validation
* Contract-first design
* Interoperability between systems

---

# What I Learned

* XML is hierarchical and strict
* XSD enforces validation rules
* Complex types define structured data
* XSD is foundational for SOAP and WSDL

