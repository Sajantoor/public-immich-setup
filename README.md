# A Barebones Template For Setting Up Immich On Your Home Server

This repository provides a template for setting up Immich on your home server and
exposing it to the internet. This utilizes no external services\* like Cloudflare,
TailScale, etc. It's designed to be a simple, secure, completely self-hosted solution.

\*Requires a SMTP services for email notifications but this is optional

## Prerequisites

- Docker
- DuckDNS or a domain name you control
- Port forwarding setup on your router for ports 80 and 443
