---
layout: post
title: "WebSocket connection failed"
date: 2023-08-12 19:00:00 +0700
categories: WebSocket
---
I am delighted that the application I built with Next.js and NestJS is now online and operational. However, it is not performing as I had anticipated. Given that it is a real-time application, I expected it to utilize websockets. Instead, it is resorting to using the fallback method of HTTP pooling.

I have exhaustively attempted various solutions from GitHub and Stack Overflow to address this persistent error:

```bash
WebSocket connection to 'wss://api.ipm.poker/socket.io/?roomId=1&EIO=4&transport=websocket&sid=dBiHvw6rmlj2U4V3AAAI' failed: 
```

In Firefox, the error message is:or in Firefox you will see:

```bash
Firefox can’t establish a connection to the server at wss://api.ipm.poker/socket.io/?roomId=1&EIO=4&transport=websocket&sid=Vdy3c9KkYXyfDFFhAAAK.
```

Furthermore, Firefox presents additional errors such as `NS_ERROR_WEBSOCKET_CONNECTION_REFUSED` and `400 Bad Request`.

Initially, I suspected a credential issue. However, after hours of thorough debugging, I could not pinpoint any problems related to credentials. My next hypothesis was centered around CORS origin, a commonly discussed topic on both Stack Overflow and SocketIO forums.

Surprisingly, CORS does not seem to be the culprit, even though I have allowed access from all domains.

To gain more insight, I added extensive logging from the front-end to the server. Strangely, all indications suggested a successful connection:

```ts
socket.on('connect', () => {
  const transport = socket.io.engine.transport.name
  console.info('transport', transport)
  console.info('connect', 'connected')

  socket.io.engine.on('upgrade', () => {
    const upgradeTransport = socket.io.engine.transport.name
    console.log('upgradeTransport', upgradeTransport)
  })
})
socket.on('disconnect', () => {
  console.info('disconnect', 'disconnect')
})
socket.on('connect_error', (err) => {
  console.error('connect_error', err)
})
socket.on('error', (err) => {
  console.error('error', err)
})
socket.on('reconnect_error', (err) => {
  console.error('reconnect_error', err)
})
socket.on('reconnect_failed', (err) => {
  console.error('reconnect_failed', err)
})
```

Curiously, even though the server logs indicated a successful connection, the issue persisted.

After extensive and lengthy debugging efforts, I finally identified the root cause of the problem: the error originated from the proxy server, specifically, Nginx. By default, Nginx does not initiate an upgrade to the connection when a protocol switch occurs.

Before making the necessary adjustments, my configuration looked like this:

```bash
location / {
    proxy_pass http://127.0.0.1:5000;
    include proxy_params;
}
```

In order to resolve this issue, the following lines should be added to the configuration:And you should add:

```bash
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```
Therefore, the updated configuration should appear as follows:So it looks like this:

```bash
location / {
    proxy_pass http://127.0.0.1:5000;
    include proxy_params;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}

```

This solution was sourced from the following references:
:
- [NGINX double proxy not allowing websocket connections](https://serverfault.com/questions/1102091/nginx-double-proxy-not-allowing-websocket-connections)
- [Problem: the socket is stuck in HTTP long-polling](https://socket.io/docs/v4/troubleshooting-connection-issues/#problem-the-socket-is-stuck-in-http-long-polling)