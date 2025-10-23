# Development

## Table of content:

- [Dev environment](#dev-environment)
- [Architecture](#architecture)
    - [Understanding the schema](#understanding-the-schema)
    - [Adding Components/Services](#adding-componentsservices)


## Dev environment

Please keep development tool specific config out of the repo as much as possible.

### OPNSense test instance

You'll want a test instance of OPNsense to write and execute tests against and inspect API requests made by the OPNsense web-ui itself.

***Setup instructions***

## Architecture

This library generates the underlying API from custom schema files located in the `schema` directory by using `go generate` directives. OPNSense appears to have a pretty consistent API for getting and setting parameters, so this approach tends to reduce code duplication.  The code used to generate the API is located in the `internal/generate` directory. 

There are two types of generated objects: individual `controllers` and the opnsense `client` itself. The `controllers` are used to support individual services/components in OPNSense and the `client` represents the service as a whole. 

### Understanding the schema

The schema is a yaml representation of the [OPNSense API here](https://docs.opnsense.org/development/api.html)

All the schema implementations can be found in the [schema.go](https://github.com/browningluke/opnsense-go/blob/d14119a35bbf0899c1e442c5d3ad194483fef033/internal/generate/schema/schema.go) file in `internal/generate/schema`.

Here is a quick overview of the basic structure each file in `schema/{API service}.yml` follows:

```yml
---
# Service name, should be the same as OPNsense api.
name: core
# Relevant when the OPNsense api has a special configure endpoint
reconfigureEndpoint: ""
# OPNSense API endpoints that generally follow a CRUD pattern
resources: []ResourceData
# Extra OPNSense API endpoints that execute special functionality
rpc: []RPCData
```

### Adding Components/Services

Components can be added by creating a `{service}.yml` schema file describing the component/service API under the `schema` directory (for now use the existing schema as a reference) and then adding a `pkg/{service}/generate.go` file. The `generate.go` file can be copied from the existing servicess.  The package can then be regenerated using `make all` in the base directory. 
