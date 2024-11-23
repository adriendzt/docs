---
title: Basic webserver
layout: default
nav_order: 1
parent: "Bash"
---

We can use netcat listener to create a simple webserver running on port 8080 (-4 for ipv4):

```bash
echo -e 'HTTP/1.1 200\r\nContent-Type: text/html\r\n\r\n<h1>PONG</h1>' | nc -4 lv 8080
```

This server will stop after the first query, if we want to run it forever, we can run it in a loop:


```bash
while true;
  do echo -e 'HTTP/1.1 200\r\nContent-Type: text/html\r\n\r\n<h1>PONG</h1>' | nc -4 lv 8080
done
```

We can run a script when a client is connecting and display this script output:

```bash
while true;
  do { echo -e 'HTTP/1.1 200\r\n'; sh web_body.sh; } | nc -4 lv 8080
done
```

The content of web_body.sh as an example, it displays the hostname and the date:

```bash
hostname
date
```