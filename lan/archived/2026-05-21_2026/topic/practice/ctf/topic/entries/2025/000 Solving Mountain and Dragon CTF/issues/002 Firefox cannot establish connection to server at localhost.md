# 1 Journal

* [x] 

From [^spawn-issue-1bfa08](002%20Firefox%20cannot%20establish%20connection%20to%20server%20at%20localhost.md#spawn-issue-1bfa08) in [3.4 Send web messages to terminal to collect experiment data](002%20Firefox%20cannot%20establish%20connection%20to%20server%20at%20localhost.md#34-send-web-messages-to-terminal-to-collect-experiment-data)

2025-07-30 Wk 31 Wed - 13:44

````
Firefox can’t establish a connection to the server at ws://localhost:3003/.
````

This happens when trying to start a client with [websockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket),

````ts
export function start_client() {
  // Create WebSocket connection.
  const socket = new WebSocket("ws://localhost:3003");

  // Connection opened
  socket.addEventListener("open", (event) => {
    socket.send("Hello Server!");
  });

  // Listen for messages
  socket.addEventListener("message", (event) => {
    console.log("Message from server ", event.data);
  });
}
````

2025-07-30 Wk 31 Wed - 13:50

This [stackoverflow answer](https://stackoverflow.com/a/60003844/6944447) suggests setting a flag in `about:config`:

````
network.dns.native-is-localhost
````

It's by default set to false.

Resetting back, didn't resolve the problem.

Oh actually it seems we can connect. I didn't realize my server was dying:

````sh
socat - TCP4-LISTEN:3003
````

Could this server itself be the issue?

2025-07-30 Wk 31 Wed - 14:03

Let's try [gh websocat](https://github.com/vi/websocat) instead to serve,

````sh
cargo install websocat
````

Serve on 3003:

````sh
websocat -s 3003
````

^websocat-serve

I works!

I noticed that with websocket the client was sending GET requests, so our basic socat server was not enough.

This should also help with this [greasemonkey task](https://github.com/LanHikari22/lan-setup-notes/blob/main/lan/topics/tooling/web/entries/2025/000%20Making%20Greasemonkey%20scripts.md#17-creating-command-that-can-be-summoned-via-console-to-retrieve-information) to create a robust command line control outside the browser and to retrieve information.
