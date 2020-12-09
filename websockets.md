##**Websockets**
***

Resources
- [a youtube video on websockets](https://www.youtube.com/watch?v=vQjiN8Qgs3c)
- [free code camp video hosted presentation](https://www.youtube.com/watch?v=8ARodQ4Wlf4)
- [MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)


Websockets
allow two-way, event driven communication between web browser and server. This API allows you to send messages to a server and receive the event-driven responses without having to poll the server for a reply

Interfaces
- WebSocket:
The primary interface for connecting to a WebSocket server and then sending and receiving data on the connection.
- CloseEvent:
The event sent by the WebSocket object when the connection closes.
- MessageEvent:
The event sent by the WebSocket object when a message is received from the server.

uses of websockets
- multiplayer browser games
- collaborative code editing
- live text for sports/news websites
- online drawing canvas
- real-time to-do apps with multiple users
- chat apps

![representation of Websockets](https://images.ctfassets.net/ee3ypdtck0rk/1u0ijAerVQdTEvV2RUGcGp/9b0b67b0014254f8af5064f63c4a81a6/websockets.png)
![representation of Websockets](https://images.ctfassets.net/3prze68gbwl1/6gIRdHedHRLNmco97gFajb/2d36a5ddfc47831ca737bbcf24e31d7c/WebSockets2.jpg)
higher level 
![browser-Websockets-server](https://portswigger.net/web-security/images/websockets.svg)


**Internet Protocol Suite**
take this with a grain of salt, very contrived
[2:00](https://www.youtube.com/watch?v=8ARodQ4Wlf4)

| {application Layer}                | {Internet Layer} | {Transport Layer} |
|------------------------------------|---------------------|-------------------|
| [HTTP, Websockets, SSL, IMAP, POP] | [IPv4, IPv6]        | [TCP, UDP]        |

{Transport Layer} - defines how the packets are transfered
{Internet Layer} - define the address the packets are sent
{application Layer} -  define the process to process of packet transfer

HTTP is a response model

Client----->request
response<-----server
