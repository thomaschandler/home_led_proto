# Home LED Protocol

This repository contains the protocol and associated tests for the Home LED
project.

## Overview

Currently this is a one way protocol used by [home_led_arduino](https://github.com/thomaschandler/home_led_arudino) and
[home_led_srv](https://github.com/thomaschandler/home_led_srv).

When uploaded to an Arduino, the `home_led_arduino` sketch reads
`ControlMessage` protocol buffers over the serial port, and uses the contained
data to set a string of WS2812 LEDs.

The protocol doesn't specify the type of LEDs that are required, so the proto
reciever could potentially be updated to drive a variety of LED strips.

## Repo Structure

- `proto`
  - Protocol Definition
  - Uses [proto3](https://developers.google.com/protocol-buffers/docs/proto3)

## Compiling

Instructions on how to compile the protocol are shown below. Currently only Go
and C are shown, as this covers all projects that currently use this protocol
(see above).

For compiling the protocol for other languages, see the documentation provided
with your protobuf library.

### Common Setup

- Follow instructions to setup [protoc](https://github.com/protocolbuffers/protobuf#protocol-compiler-installation)
  - `protoc` might be bundled with a pacakage manager
    - OSX: `brew install protobuf`

### Go

The Go protobuf implementation can be found [here](https://github.com/golang/protobuf).

For the example below, the output directory is selected such that
`home_led_srv` can be built. Should a different output directory be required,
change `--go_out=` to the desired location.

In order for the following steps to work, make sure `$GO_PATH` is set
correctly. As an example: `export GO_PATH=~/go`

```
go get -u github.com/golang/protobuf/protoc-gen-go
go build github.com/golang/protobuf/protoc-gen-go
export PATH=$GO_PATH/bin:$PATH
# The following command will mention "no Go files". This warning can be
# disregarded
go get -u github.com/thomaschandler/home_led_proto
cd $GO_PATH/src/github.com/thomaschandler/home_led_proto
# Assumes Common Setup (above) is complete and protoc exists in $PATH
protoc --go_out="." proto/led.proto
```

### C

The library used for the C-side implementation is [nanopb](https://github.com/nanopb/nanopb).
