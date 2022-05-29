

#C2 with Reverse-Poxy in Docker & detection with WireShark

We'll create three containers, an Attacker, a Reverse-Proxy, and a Victim. 👹🤖🤔
We'll configure the reverse-proxy to serve a harmless website and proxy requests to our hidden attacker server. 😇😈
We'll use a RAT, remote access tool, to control the victim. 🐀💀
We'll capture packets and analyze them. 🔍🤔


So the reverse proxy will hide our C2 traffic, an encrypted Merlin session and accessible through normal web to limit the risk of detection.
//So delete the part about a phishing site or whatever, just put in to paste the execution code into the victim (we'll not be covering techniques on how to get them to execute) and now we have an encrypted connection to a seemingly normal website that passes the traffic to us, the Attacker.
So first! Do merlin without reverse proxy and read from WireShark!
Second make the reverse proxy so that makes it harder to read!

then how to detect hidden c2 traffic.
beacon analysis