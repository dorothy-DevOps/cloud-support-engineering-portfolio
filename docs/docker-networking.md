# Docker Networking

## 1. Overview

Docker networking enables containers to communicate with one another, communicate with the host machine, and access external networks.

In this practical exercise, I learned how to create a user-defined bridge network, connect containers to it, test container-to-container communication, understand Docker DNS resolution, and troubleshoot network connectivity.

## 2. Learning Objectives

By completing this practical, I learned how to:

- Understand Docker network types, particularly bridge networks.
- Create and inspect a user-defined bridge network.
- Connect containers to a shared network.
- Test communication between containers using container names.
- Understand Docker's embedded DNS resolver.
- Identify container IP addresses, network subnets, and gateways.
- Disconnect and reconnect containers to a network.
- Troubleshoot container connectivity issues.

## 3. Inspecting Existing Docker Networks

Before creating a network, I listed the existing Docker networks:

```bash
docker network ls
```

Docker displayed three default networks:

| Network | Driver | Purpose |
|---|---|---|
| bridge | bridge | Default network for containers |
| host | host | Uses the host network directly |
| none | null | Provides no network connectivity |

### 3.1 Inspecting the Default Bridge Network

I used:

```bash
docker network inspect bridge
```

This helped me understand the network configuration, including:

- Network driver
- Subnet
- Gateway
- Connected containers

The default bridge network showed:

- Subnet: `172.17.0.0/16`
- Gateway: `172.17.0.1`

### 3.2 Creating a User-Defined Bridge Network

I created a dedicated network for my application containers:

```bash
docker network create dorothy-app-network
```

I then inspected it:

```bash
docker network inspect dorothy-app-network
```

The network configuration showed:

| Setting | Value |
|---|---|
| Network name | `dorothy-app-network` |
| Driver | `bridge` |
| Scope | `local` |
| Subnet | `172.18.0.0/16` |
| Gateway | `172.18.0.1` |
| Internal | `false` |

A user-defined bridge network allows connected containers to communicate using their container names through Docker's embedded DNS.

## 4. Creating and Connecting Containers

I created two containers and attached both to `dorothy-app-network`.

### 4.1 Creating the Nginx Web Container

```bash
docker run -d \
  --name dorothy-web \
  --network dorothy-app-network \
  nginx
```

This created a running Nginx web server container.

I did not publish a host port because the objective was to test communication between containers on the same Docker network.

### 4.2 Creating the BusyBox Client Container

```bash
docker run -d \
  --name dorothy-client \
  --network dorothy-app-network \
  busybox:1.36 \
  sleep 3600
```

BusyBox provided a lightweight environment for network testing.

The `sleep 3600` command kept the container running for one hour so I could execute diagnostic commands inside it.

### 4.3 Inspecting the Network

```bash
docker network inspect dorothy-app-network
```

The inspection confirmed that both containers were connected.

Their assigned IP addresses were:

| Container | IP address |
|---|---|
| `dorothy-web` | `172.18.0.2` |
| `dorothy-client` | `172.18.0.3` |

### 4.4 Testing Container-to-Container Communication

I executed the following command:

```bash
docker exec dorothy-client wget -qO- http://dorothy-web
```

The command returned the Nginx welcome page HTML.

This demonstrated that:

- Both containers could communicate over their shared network.
- Docker's embedded DNS resolved `dorothy-web` to its container IP address.
- Host port publishing was not required for this container-to-container request.

## 5. Understanding Docker DNS and IP Routing

### 5.1 Testing Docker DNS Resolution

I used:

```bash id="2fc6nv"
docker exec dorothy-client nslookup dorothy-web
```

The output showed:

```text
Server: 127.0.0.11
Address: 127.0.0.11:53

Name: dorothy-web
Address: 172.18.0.2
```

This demonstrated that Docker's embedded DNS resolver (`127.0.0.11`) successfully resolved the container name `dorothy-web` to `172.18.0.2`.

Port `53` is the standard DNS port.

### 5.2 Understanding the Container's Routing Table

I inspected the routing table inside `dorothy-client`:

```bash id="rcpx57"
docker exec dorothy-client ip route
```

The output was:

```text
default via 172.18.0.1 dev eth0
172.18.0.0/16 dev eth0 scope link src 172.18.0.3
```

**Understanding the output:**

| Value | Meaning |
|---|---|
| `172.18.0.1` | Docker network gateway |
| `172.18.0.0/16` | Network subnet |
| `172.18.0.3` | IP address of `dorothy-client` |
| `eth0` | Container network interface |

The default route sends traffic destined for networks outside the directly connected subnet through the gateway.

Containers communicating within the same directly connected subnet can communicate directly without routing through the gateway.

### 5.3 DNS Resolver vs Network Gateway

These two addresses serve different purposes:

- `127.0.0.11`: Docker's embedded DNS resolver, which resolves container names.
- `172.18.0.1`: The gateway of the user-defined bridge network, which provides a route to external networks.

## 6. Disconnecting and Reconnecting Containers

I tested how disconnecting a container from a Docker network affects communication.

### 6.1 Disconnecting the Client Container

I disconnected `dorothy-client` from the network:

```bash
docker network disconnect dorothy-app-network dorothy-client
```

I then inspected the network:

```bash
docker network inspect dorothy-app-network
```

The `Containers` section showed only `dorothy-web`. This confirmed that `dorothy-client` was no longer attached to the network.

### 6.2 Testing Communication After Disconnection

I attempted the same communication test:

```bash
docker exec dorothy-client wget -qO- http://dorothy-web
```

The result was:

```text
wget: bad address 'dorothy-web'
```

This happened because `dorothy-client` was disconnected from the user-defined network and could no longer resolve `dorothy-web` through that network's embedded DNS.

### 6.3 Reconnecting the Client Container

I restored the connection:

```bash
docker network connect dorothy-app-network dorothy-client
```

I repeated the communication test:

```bash
docker exec dorothy-client wget -qO- http://dorothy-web
```

The Nginx welcome page HTML appeared again.

This confirmed that reconnecting the client restored its ability to resolve and communicate with the web container.

### 6.4 Troubleshooting Lesson

A running container does not automatically have access to every Docker network.

When troubleshooting communication between application containers, I should first verify that both containers are attached to the appropriate network.

Other checks may include DNS resolution, listening ports, firewall rules, and application configuration.

## 7. Key Lessons Learned

Through this practical exercise, I learned that:

1. Docker user-defined bridge networks allow containers to communicate with one another.
2. Docker embedded DNS allows containers on the same user-defined network to communicate using container names instead of manually specified IP addresses.
3. A container's IP address, network gateway, and DNS resolver serve different purposes.
4. Containers on the same directly connected subnet can communicate without routing through the network gateway.
5. Disconnecting a container from a network affects its ability to communicate with other containers on that network.
6. Reconnecting the container restores network connectivity and name-based communication.

## 8. Practical Environment

| Component | Configuration |
|---|---|
| Operating system | Windows 11 with WSL2 Ubuntu |
| Container platform | Docker Desktop |
| Network | `dorothy-app-network` |
| Network driver | Bridge |
| Web container | `dorothy-web` (Nginx) |
| Client container | `dorothy-client` (BusyBox 1.36) |
| Subnet | `172.18.0.0/16` |
| Gateway | `172.18.0.1` |
| DNS resolver | `127.0.0.11` |