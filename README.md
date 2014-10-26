cec-web
=======

A REST micro webservice to control devices via the CEC bus in HDMI.

Written in Go with some help from [Gin](http://gin-gonic.github.io/gin/), [Go-Flags](https://github.com/jessevdk/go-flags) and [cec.go](https://github.com/chbmuc/cec).

Usage
=====

    Usage:
      cec-web [OPTIONS]
    
    Application Options:
      -i, --ip=      ip to listen on (127.0.0.1)
      -p, --port=    tcp port to listen on (8080)
      -a, --adapter= cec adapter to connect to [RPI, usb, ...]
      -n, --name=    OSD name to announce on the cec bus (REST Gateway)


JSON API
========

The app provides the following JSON based RESTful API:

## Scan CEC bus

* ``GET /info`` - Information about all the connected devices on the CEC bus

#### Resonse

    HTTP/1.1 200 OK

```json
{
  "Playback":{
    "OSDName":"REST Gateway",
    "Vendor":"Panasonic",
    "LogicalAddress":4,
    "ActiveSource":false,
    "PowerStatus":"on",
    "PhysicalAddress":"f.f.f.f"
  },
  "TV":{
    "OSDName":"TV",
    "Vendor":"Panasonic",
    "LogicalAddress":0,
    "ActiveSource":false,
    "PowerStatus":"standby",
    "PhysicalAddress":"0.0.0.0"
  }
}
```
    
## Power

* ``GET /power/:device`` - Request device power status
* ``PUT /power/:device`` - Power on device
* ``DELETE /power/:device`` - Put device in standby

``:device`` is the name of the device on the CEC bus (see ``GET /info``)

#### Responses

success (PUT/DELETE); is powered on (GET)

    HTTP/1.1 204 No Content

is in standy/no power (GET)

    HTTP/1.1 404 Not Found


## Volume (not supported by all devices)

* ``PUT /volume/up`` - Increase volume
* ``PUT /volume/down`` - Reduce volume
* ``PUT /volume/mute`` - Mute/unmute audio

#### Response

    HTTP/1.1 204 No Content

## Remote control

* ``PUT /key/:device/:key`` - Send key press command followed by key release

``:device`` is the name of the device on the CEC bus (see ``GET /info``)

``:key`` is the name (e.g. ``down``) or the keycode in hex (e.g. ``0x00``) of a remote key

#### Response

    HTTP/1.1 204 No Content

## Raw CEC commands

* ``POST /transmit`` - Send a list of CEC commands over the bus

data example:
```json
[
  "40:04",
  "40:64:00:48:65:6C:6C:6F:20:77:6F:72:6C:64"
]
```

Hint: Use [cec-o-matic](http://www.cec-o-matic.com/) to generate commands.
