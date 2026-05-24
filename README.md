# 2(c) SIMULATING ARP / RARP PROTOCOLS

## AIM

To write a Python program for simulating ARP and RARP protocols using TCP sockets.

---

# ALGORITHM

## Client

1. Start the program.
2. Establish socket connection between client and server.
3. Get the IP address (for ARP) or MAC address (for RARP) from the user.
4. Send the address to the server.
5. Receive the corresponding MAC/IP address from the server.
6. Display the result.

---

## Server

1. Start the program.
2. Create and bind the socket.
3. Listen and accept the client connection.
4. Maintain a table containing IP and MAC address mappings.
5. Receive the address sent by the client.
6. Search the table for the corresponding value.
7. Send the mapped address back to the client.

---

# PROGRAM – ARP

## Server Program (ARP)

```python
# arp_server.py

import socket

arp_table = {
    "192.168.1.1": "AA:BB:CC:DD:EE:01",
    "192.168.1.2": "AA:BB:CC:DD:EE:02",
    "192.168.1.3": "AA:BB:CC:DD:EE:03"
}

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("localhost", 5000))
server.listen(1)

print("ARP Server Waiting for Connection...")

conn, addr = server.accept()
print("Connected by", addr)

ip = conn.recv(1024).decode()

mac = arp_table.get(ip, "MAC Address Not Found")

conn.send(mac.encode())

conn.close()
server.close()
```

---

## Client Program (ARP)

```python
# arp_client.py

import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("localhost", 5000))

ip = input("Enter IP Address: ")

client.send(ip.encode())

mac = client.recv(1024).decode()

print("MAC Address:", mac)

client.close()
```

---

# OUTPUT – ARP

```text
Server Side:
ARP Server Waiting for Connection...
Connected by ('127.0.0.1', 54321)

Client Side:
Enter IP Address: 192.168.1.2
MAC Address: AA:BB:CC:DD:EE:02
```

---

# PROGRAM – RARP

## Server Program (RARP)

```python
# rarp_server.py

import socket

rarp_table = {
    "AA:BB:CC:DD:EE:01": "192.168.1.1",
    "AA:BB:CC:DD:EE:02": "192.168.1.2",
    "AA:BB:CC:DD:EE:03": "192.168.1.3"
}

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("localhost", 6000))
server.listen(1)

print("RARP Server Waiting for Connection...")

conn, addr = server.accept()
print("Connected by", addr)

mac = conn.recv(1024).decode()

ip = rarp_table.get(mac, "IP Address Not Found")

conn.send(ip.encode())

conn.close()
server.close()
```

---

## Client Program (RARP)

```python
# rarp_client.py

import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("localhost", 6000))

mac = input("Enter MAC Address: ")

client.send(mac.encode())

ip = client.recv(1024).decode()

print("IP Address:", ip)

client.close()
```

---

# OUTPUT – RARP

```text
Server Side:
RARP Server Waiting for Connection...
Connected by ('127.0.0.1', 54322)

Client Side:
Enter MAC Address: AA:BB:CC:DD:EE:03
IP Address: 192.168.1.3
```

---

# RESULT

Thus, the Python program for simulating ARP and RARP protocols using TCP sockets was successfully executed.
