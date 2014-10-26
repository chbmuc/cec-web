cec-web
=======

A REST micro webservice to control devices via the CEC bus in HDMI.

Written in Go with some help from Gin(http://gin-gonic.github.io/gin/), Go-Flags(https://github.com/jessevdk/go-flags) and cec.go(https://github.com/chbmuc/cec).

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

Information about all the connected devices on the CEC bus:

    GET /info

### Resonse

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
    

