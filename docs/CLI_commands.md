# Useful CLI commands

## Setting up the network

```
plugin network-creator start 1
```
```
plugin network-creator-security open-network
```
```
plugin network-steering start 0
```

## Using preset commands

<https://docs.silabs.com/zigbee/latest/af/group-on-off>

**zcl on-off toggle** Command description for Toggle

**zcl on-off on** Command description for On

**zcl on-off off** Command description for Off

<https://docs.silabs.com/zigbee/latest/af/group-zdo>

**zdo ieee node_ID** Request an ieee address based on a given node id

**zdo nwk ieee:8** Sends a network address request for the given IEEE address.

## Set up a binding between devices

**option binding-table set index cluster local ep remote ep EUI**

> option binding-table set 0 0x0006 1 1 { 588E81FFFE72FE72 }

> option binding-table set 0 0x0000 1 1 { 000B57FFFE07383B }

<https://docs.silabs.com/zigbee/6.3/af_v2/group-zcl-global>

**zcl global send-me-a-report [cluster:2] [attributeId:2] [dataType:1] [minReportTime:2] [maxReportTime:2] [reportableChange:-1]**

> zcl global send-me-a-report 0x0006 0x0000 0x10 1 15 { 00 }

> zcl global send-me-a-report 0x0000 0xFFFD 0x21 1 15 { 00 }

Send a pre-buffered message from a given endpoint to an endpoint on a device with a given short address.

**send [id:2] [src-endpoint:1] [dst-endpoint:1]**

> send 0x9AB1 1 1

- id - INT16U - short id of the device to send the message to
- src-endpoint - INT8U - The endpoint to send the message from
- dst-endpoint - INT8U - The endpoint to send the message to

## Generating your own commands

Creates a global read command message to read from the cluster and attribute specified

**zcl global read [cluster:2] [attributeId:2]**

> zcl global read 0x0006 0x0000

- cluster - INT16U - The cluster id of the cluster to read from
- attributeId - INT16U - The attribute id of the attribute to read.

Create a global write command message to write to the cluster and attribute specified

**zcl global write [cluster:2] [attributeId:2] [type:4] [data:-1]**

> zcl global write 0x0006 0x0000 0x10 {01}

- cluster - INT16U - The cluster id of the cluster to write to.
- attributeId - INT16U - The attribute id of the attribute to write (see attribute_type.h for details)
- type - INT32U - The type of the attribute to write.
- data - OCTET_STRING - The data to be written.

Read an attribute from the local (ie on the same device) attribute table. The attribute is displayed on the command line.

**read [endpoint:1] [cluster:2] [attribute:2] [mask:1]**

> read 1 0x0006 0x0000 1

- endpoint - INT8U - endpoint of the attribute to read
- cluster - INT16U - cluster id of the attribute to read
- attribute - INT16U - attribute id of the attribute to read
- mask - INT8U - direction mask of the attribute to read (client=0 or server=1)

Write an attribute value into the local attribute table

**write [endpoint:1] [cluster:2] [attribute:2] [mask:1] [dataType:1] [dataBytes:-1]**

> write 1 0x0006 0x0000 1 0x10 {0}

- endpoint - INT8U - endpoint of the attribute to write
- cluster - INT16U - cluster id of the attribute to write
- attribute - INT16U - attribute id of the attribute to write
- mask - INT8U - direction mask of the attribute to write (client=0 or server=1)
- dataType - INT8U - the attribute type as listed in the generated file attribute-type.h
- dataBytes - OCTET_STRING - string of bytes you wish to write into the attribute table.

## Sending Leave commands

**plugin test-harness z3 mgmt leave [destination:2] [removeChildren:1] [rejoin:1] [options:4]**

- destination - INT16U - The destination address of the command.
- removeChildren - BOOLEAN - Whether or not the leaving device should remove its children.
- rejoin - BOOLEAN - Whether or not the destination node should rejoin the network.
- options - INT32U - The negative behavior options for this command.

**plugin test-harness z3 nwk nwk-leave [rejoin:1] [request:1] [removeChildren:1] [destination:2] [options:4]**

- rejoin - BOOLEAN - Whether or not the device should rejoin.
- request - BOOLEAN - Whether or not this command is a request.
- removeChildren - BOOLEAN - Whether or not the leaving device should remove its children.
- destination - INT16U - The destination address of the command.
- options - INT32U - The negative behavior options for this command.
