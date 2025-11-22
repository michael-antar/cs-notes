A device (hardware) or software (like Nginx or AWS ELB) that sits in front of your server farm.

**What it does**:
1. **Traffic Distribution**: It has a *list of IP Addresses* (Server A, Server B, Server C). When a request comes in, it picks one (usually via "Round Robin" or "Least Connections") and forwards the packet.
2. **Health Checks**: It pings every server every few seconds. If Server B doesn't reply, the LB crosses it off the list and stops sending traffic there.
3. **SSL Termination**: It often handles the heavy math of *decrypting HTTPS traffic* so your backend servers don't have to.

It _DOES NOT_ start new servers. If all servers are at 100% CPU, the Load Balancer just keeps sending traffic to them (or drops requests). It does not have the power to create new machines.