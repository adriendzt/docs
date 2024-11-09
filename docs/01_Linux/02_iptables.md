---
title: IP tables
layout: default
nav_order: 2
parent: "Linux"
---

Iptables can be used to reject the connection from a source or to create a NAT mapping

## List the rules

To check the current rules: `iptables -L -n --line-number`\
Drop a specific line in the iptables in the FORWARD section: `iptables -D FORWARD 5`\

## Reject a connection

Add a rule to reject a connection: `iptables -A INPUT -s 192.168.1.4 -j REJECT`\
Drop this rule: `iptables -D INPUT -s 192.168.1.4 -j REJECT`

## Create a NAT rule

Add a NAT mapping: `iptables -t nat -A PREROUTING -i br3 -p tcp --dport 8181 -j DNAT --to 192.168.1.4:8181`\
The port 8181 of your interface br3 is mapped to 192.168.1.4 port 8181
