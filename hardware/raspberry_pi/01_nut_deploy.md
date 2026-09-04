```shell
docker run -d \
  --name nut \
  --restart always \
  --privileged \
  -v /dev/bus/usb:/dev/bus/usb \
  -e UPS_NAME=apc_ups \
  -e UPS_DRIVER=usbhid-ups \
  -e UPS_PORT=auto \
  -e API_USER=thelastcookies \
  -e API_PASSWORD=Nut720611,. \
  -p 3493:3493 \
  instantlinux/nut-upsd:latest
```
