# Hostname: "Panda" docker images

---

## Access

### authentik

This Docker image provides authentication for all required Docker images and other OAuth integrations. It serves as a centralized identity provider for managing user access via OAuth2, OpenID Connect, SAML, and LDAP, ensuring secure and streamlined authentication across services.

---

## Apps

### diun

Diun is a Docker container that automatically monitors your running Docker images and notifies you when an update is available. It helps in keeping your Docker containers up-to-date by sending alerts through various channels like email, Slack, or webhooks whenever a new version is released.

### dozzle

Dozzle is a lightweight, real-time log viewer for Docker containers. It allows you to stream and monitor logs from your running Docker containers via a simple web interface. Ideal for quick debugging, Dozzle helps you easily observe logs without the need to SSH into the host machine.

### homarr

Homarr is a customizable, web-based dashboard designed for managing and monitoring self-hosted applications. It provides a central hub where you can organize links to your running services, monitor their status, and integrate various features like notifications, making your home server or self-hosted environment more user-friendly.

### immich

Immich is a self-hosted platform for efficient and automatic photo and video backup from your mobile devices. It provides secure storage, seamless synchronization, and organization for your media, all hosted within your infrastructure, giving you full control over your data.

### uptimekuma

Uptime Kuma is a self-hosted monitoring tool designed to keep track of the uptime and availability of websites, APIs, and services. It provides real-time status updates and can notify you through various channels when services go down, ensuring you're aware of any disruptions in your infrastructure.

### vscode

The VSCode Docker image provides a development environment with Visual Studio Code, a powerful code editor. This image is ideal for remote development, allowing you to write, debug, and manage code inside containers with all the features of VSCode, such as syntax highlighting, code completion, and integrated version control.

  

## Media

### Bazarr

Bazarr is a companion tool for Sonarr and Radarr that automatically downloads subtitles for your movies and TV shows. It integrates with multiple subtitle providers, ensuring that your media library is properly subtitled in various languages.

### Emby

Emby is a media server that helps you organize, stream, and manage your personal media library. It provides access to your content across devices, offering features like live TV, DVR, and mobile sync, with a focus on privacy and user control.

### FlareSolverr

FlareSolverr is a proxy server that acts as a bridge to bypass Cloudflare’s anti-bot protection. It is typically used with automation tools like Prowlarr, Sonarr, and Radarr to scrape websites without being blocked by Cloudflare’s security checks.

### Gluetun

Gluetun is a privacy-focused VPN client container that routes your Docker traffic through VPN services. It supports multiple VPN providers and ensures that your containers are securely connected, offering DNS over HTTPS and other privacy features to protect your browsing and download activities.

### Jellyfin

Jellyfin is a free, open-source media server that allows you to organize, stream, and access your personal media from anywhere. It’s similar to Emby and Plex, but with a strong focus on privacy and without the need for subscriptions or external cloud dependencies.

### Jellyseerr

Jellyseerr is a web-based companion to Jellyfin, providing a user-friendly interface for managing media requests. It allows users to search and request new content (movies or TV shows), streamlining the process of adding media to your Jellyfin library through integrations with automation tools like Radarr and Sonarr.

### Prowlarr

Prowlarr is an indexer manager for use with Sonarr, Radarr, and other *arr applications. It aggregates and manages indexers from multiple torrent and Usenet providers, streamlining the search for content to automate downloads via these apps.

### qBittorrent

qBittorrent is a lightweight, open-source torrent client that provides a feature-rich and easy-to-use interface for managing torrent downloads. It supports automation, RSS feeds, and can be integrated with tools like Sonarr and Radarr to manage media downloads directly.

### Radarr

Radarr is a movie collection manager that automates the process of searching, downloading, and organizing movies. It integrates with torrent and Usenet clients to handle the complete movie acquisition process and ensures your movie library is always up-to-date.

### SABnzbd

SABnzbd is an open-source Usenet downloader designed for automatic downloading of content from Usenet. It works with tools like Sonarr and Radarr to manage and process Usenet downloads in a seamless, user-friendly manner.

### Sonarr

Sonarr is a PVR (Personal Video Recorder) for automatically downloading TV shows. It integrates with indexers and download clients (like qBittorrent or SABnzbd) to automate the process of searching, grabbing, and organizing episodes in your TV show library.

  

---

## Network