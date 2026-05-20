# A Barebones Template For Setting Up Immich On Your Home Server

This repository provides a template for setting up Immich on your home server and
exposing it to the internet. This utilizes no external services\* like Cloudflare,
TailScale, etc. It's designed to be a simple, secure, completely self-hosted solution.

\*Requires a SMTP services for email notifications but this is optional

## Prerequisites

- Docker
- DuckDNS or a domain name you control
- Port forwarding setup on your router for ports 80 and 443

## Setting up DuckDNS

1. Go to [DuckDNS](https://www.duckdns.org/) and create an account.
1. Add a new subdomain (e.g., `myhome.duckdns.org`).
1. Note down your DuckDNS token and the subdomain you created.
1. Update your router to forward ports 80 and 443 to the internal IP address of your home server
1. Update your DynDNS IPV4 to use DuckDNS, using the domain and token in step 3.

## Architectural Overview

The architecture consists of the following components:

1. **Traefik** - A reverse proxy that handles incoming requests, provides SSL termination, and routes traffic to the appropriate services. Traefik was chosen since it's easy to configure with Docker, but other reverse proxies like Nginx or Caddy could be used as well.
2. **Crowdsec** - A security tool that provides protection against various types of attacks. It is integrated with Traefik to block malicious traffic.
3. **Authelia** - A lightweight authentication and authorization server that provides things like 2FA and SSO using OIDC.

Requests flow as follows:

Traefik -> Crowdsec (Security) -> Authelia (Authentication) -> Immich (Application)

## Getting Started

1. Copy the `.env.template` file to `.env` and update the environment variables
1. Copy the `.env.template` file in apps/immich to `apps/immich/.env` and update the environment variables
1. Run `docker compose up -d` to start the services
1. Update the `CROWDSEC_BOUNCER_API_KEY` in the `.env` file by running the following command, copy the key:
   `docker exec crowdsec cscli bouncers add traefik-bouncer`
1. Access Immich at your desired domain

## Updating the Services

1. Run `docker compose pull` to pull the latest images
1. Run `docker compose up -d` to restart the services with the new images

Note: This should be done periodically to ensure you have the latest security patches and features,
but be aware that it will cause some small downtime while the services restart.

## You Should Still Have Backups

https://docs.immich.app/administration/backup-and-restore/
