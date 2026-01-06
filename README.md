# Unfi-controller-and-cloudflare-docker-compose
This docker compose is consist of MongoDB / Unifi Controller along with the Cloudflared to integrate with the cloudflare tunnels
# UniFi Network Application: Secure Docker Deployment
#Zero Trust Architecture with Cloudflare Tunnel

This repository provides a **security-first, Zero-Trust Docker Compose deployment** of the **UniFi Network Application**. By leveraging Cloudflare Tunnels, this setup eliminates the need for port forwarding and keeps your controller hidden from the public internet.

---

#Key Features

* **Zero Inbound Ports:** No ports are exposed to the host machine.
* **Encapsulated Stack:** UniFi UI is accessible *only* through Cloudflare Zero Trust.
* **Bypass Restrictions:** Works perfectly behind CGNAT, Starlink, or restrictive corporate firewalls.
* **Enterprise-Grade Security:** Layer Cloudflare Access (SSO/MFA) on top of your UniFi login.
* **Isolated Database:** MongoDB 7 running in a private internal Docker network.

---

# Architecture Overview



The traffic flow is strictly one-way (outbound) from your origin to Cloudflare:

graph TD
    User((User)) --> CF_Access[Cloudflare Access SSO/MFA]
    CF_Access --> CF_Tunnel[Cloudflare Edge]
    CF_Tunnel -->|Encrypted Tunnel| cloudflared[cloudflared Container]
    cloudflared -->|Internal Network| UniFi[UniFi Controller]
    UniFi -->|Internal Network| Mongo[(MongoDB 7)
    
Internal Networking: All services communicate via a private Docker bridge network.Service Discovery: Docker DNS resolves service names internally.Hardened Perimeter: The UniFi UI is never directly exposed to your local network or the internet.📁 Project StructurePlaintext.
├── docker-compose.yml    # Main orchestration file
├── init-mongo.js         # MongoDB initialization script
├── config/               # Persistent UniFi configuration & logs
└── mongodb/              # Persistent MongoDB data storage

