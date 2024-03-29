---
layout: default
title: "Confluent Schema Registry"
parent: "Integrations"
---

# Confluent Schema Registry
{: .no_toc }

1. TOC
{:toc}

## Connecting

### CLI

```bash
recap ls http+csr://my-registry:8081/some/root/path
recap schema http+csr://my-registry:8081/some/root/path/my-topic
```

### Python API

```python
from recap.clients import create_client

with create_client("http+csr://my-registry:8081/some/root/path") as client:
    client.ls()
    client.schema("my-topic")
```

## URLs

Recap's [Confluent Schema Registry](https://docs.confluent.io/platform/current/schema-registry/index.html) client takes a URL pointing to the Confluent Schema Registry HTTP server and an optional subject path.

```
http+csr://[host]:[port]/[path*]/[subject]
```

You may optionally include `-key` or `-value` at the end of the subject name to specify the key or value schema, respectively. If no suffix is supplied `-value` is assumed.

{: .note }
The scheme must be `http+csr` or `https+csr`. The `+csr` suffix is required to distinguish this client from other clients that also use HTTP connections (similar to [SQLAlchemy's `dialect+driver` format](https://docs.sqlalchemy.org/en/20/core/engines.html#database-urls))

## Type Conversion

`ConfluentRegistryClient.get_schema()` uses the `AvroConverter`, `JSONSchemaConverter`, and `ProtobufConverter` classes to convert schemas, based on their type. 

Please see the individual documentation for these classes for information on how they convert types:

- Avro: [AvroConverter]({{site.baseurl}}/docs/converters/avro)
- JSON schema: [JSONSchemaConverter]({{site.baseurl}}/docs/converters/json-schema)
- Protocol Buffers: [ProtobufConverter]({{site.baseurl}}/docs/converters/protobuf)

## Limitations and Constraints

1. ConfluentRegistryClient does not support [schema references](https://docs.confluent.io/platform/current/schema-registry/fundamentals/serdes-develop/index.html#schema-references).

The conversion functions raise a `ValueError` exception if the conversion is not possible.
