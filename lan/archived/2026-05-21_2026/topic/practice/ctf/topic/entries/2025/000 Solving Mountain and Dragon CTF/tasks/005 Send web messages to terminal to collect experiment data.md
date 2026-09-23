# 1 Journal

* [x] Send a "Hello world" over port 4567 to be received in terminal from typescript.

In order to do [3.3 Build a frequency map of the commands being run and tape locations being hit on idle](005%20Send%20web%20messages%20to%20terminal%20to%20collect%20experiment%20data.md#33-build-a-frequency-map-of-the-commands-being-run-and-tape-locations-being-hit-on-idle), we need to be able to gather the data into a csv file. But right now our app can only write to the DOM or to console.log.

2025-07-30 Wk 31 Wed - 12:36

An option:

* [zeromq typescript library](https://www.npmjs.com/package/zeromq) [(on github.io)](https://zeromq.github.io/zeromq.js/).

Trying to run the pub/sub example there.

Spawn [4.2 Errors while attempting to import zeromq for non-node platform](005%20Send%20web%20messages%20to%20terminal%20to%20collect%20experiment%20data.md#42-errors-while-attempting-to-import-zeromq-for-non-node-platform) ^spawn-issue-d4b55b

2025-07-30 Wk 31 Wed - 13:12

Let's try another more basic option. ZeroMQ requires node...

Let's try the [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) example,

Start the server over port 3003:

````sh
socat - TCP4-LISTEN:3003
````

then run the client in the game

````js
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
````

2025-07-30 Wk 31 Wed - 13:40

Spawn [4.3 Firefox cannot establish connection to server at localhost](005%20Send%20web%20messages%20to%20terminal%20to%20collect%20experiment%20data.md#43-firefox-cannot-establish-connection-to-server-at-localhost) ^spawn-issue-1bfa08

2025-07-30 Wk 31 Wed - 23:29

We can find the different events in the [docs here](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close_event): close, error, message, and open.

Start a [websocket server](005%20Send%20web%20messages%20to%20terminal%20to%20collect%20experiment%20data.md#websocat-serve) over `localhost:3003` gathering experimental data to file `a`:

````sh
websocat -s 3003 > a
````

Connect to `localhost:3003`:

````ts
var g_socket = webio.connect("localhost", 3003, (event) => {
  console.log(`Server: ${event.data}`)
})
````

And begin the experiment:

````ts
  var i = 0;
  
  while (g_exit_code == 0) {
    g_chr = get_and_adv_tape();

    var cmd_idx = Math.floor(g_chr / 2);

    if (webio.m_connected) {
		g_socket.send(`${i++},${cmd_idx},${g_data_cur},${g_chr}`)
    }

    var fn = g_unk_cmds[cmd_idx];
    if (fn) fn();
  }
````

Interestingly from the data, this always loops 24 times (i=0 -> i=23) and then `g_exit_code` changes.

But it does not change anywhere at the beginning of the command, so

````ts
g_socket.send(`${i++},${cmd_idx},${g_data_cur},${g_chr},${g_exit_code}`)
````

would show all zeros for `g_exit_code`. It likely happens due to the result of the 24th command.
