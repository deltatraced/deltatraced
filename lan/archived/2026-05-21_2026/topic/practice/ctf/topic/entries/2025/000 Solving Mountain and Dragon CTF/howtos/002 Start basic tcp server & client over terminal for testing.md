# 1 Journal

* [x] 

From [this superuser answer](https://superuser.com/a/392518/2972491) which also refers to [socat examples](http://www.dest-unreach.org/socat/doc/socat.html#EXAMPLES),

````sh
sudo apt-get install socat
````

````sh
# start a TCP server and read/write data on port 3003
socat - TCP4-LISTEN:3003

# start a TCP client connection and read/write data on port 3003
nc localhost 3003
````

Note for websocket communication, you can set websocat. This is an example of a websocat server:

[^websocat-serve](002%20Start%20basic%20tcp%20server%20&%20client%20over%20terminal%20for%20testing.md#websocat-serve)
