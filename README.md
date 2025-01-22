# roborio-usb-linux

This bash script allows you to connect a roborio to a linux computer over usb using simple-dhcpd
https://github.com/javier-lopez/learn/blob/master/python/tools/simple-dhcpd

Its written for personal use, so don't expect any documentation / support

```
configure() {
	DEVICE=$(ip a | grep enp0 | sed -e 's/.*: \(.*\):.*/\1/')
	echo "Device is $DEVICE"
	ip addr add 172.22.11.1/24 dev $DEVICE
	python2 simple-dhcpd -a 172.22.11.1 -i $DEVICE -f 172.22.11.2 -t 172.22.11.2 > /dev/null &
}

configure

while true
do
	if ping -c 1 172.22.11.2 &> /dev/null
	then
		echo "ping to 172.22.11.2 successful"
	else
		echo "ping to 172.22.11.2 failed... attempting to reconfigure"
		configure
	fi
	sleep 1
done
```
