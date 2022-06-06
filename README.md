# Docker media server

## Containers

- Traefik - reverse proxy (SSL is setup allready)
- Jellyfin - media server
- Sonnarr - tv downloader
- Radarr - movie downloader
- 
- Ombi - requester for radarr & sonarr
- Deluge - torrent client, required for downloaders to work
- Jackett - Torrent tracker api, required for downloaders to work



## Step by Step

1. Clone this repo

2. Copy the example.env and give it your own variables
   
   ```bash
   cp example.env .env
   nano .env
   ```
   
3. Go to Jellyfin folder and execute 
    ```bash
    chmod -R 777 .
   ```
   
4. Create the plugin, movie and tv show folders
    ```bash
   mkdir -p jellyfin/config jellyfin/cache sonarr radarr ombi deluge jackett ${DLDIR}/completed ${DLDIR}/incomplete ${MOVIESDIR} ${TVDIR}
   ```
   
5. Sonarr and Radarr needs to be able to access the content folder
    ```bash
    chmod -R 0777 content/ 
   ```
   
6. Setup your DNS so that all `*.<your-domain>` go to the media server. Your domain doesn't have to be registered since it will only be used internally
   
7. Fire up the docker containers
   
   ```bash
   # Make sure you're running this command from inside the docker-based-media-server folder and that the .env file is inside the same folder
   sudo docker-compose up -d
   # The command will take a bit, after it runs you can run the following to check all the containers are running
   sudo docker-compose ps
   ```
 
8. If this is all setup correctly visiting your domain or the ip address of your machine should open Heimdall, and `<your-ip>:8112` or `deluge.<your-domain>` should open the Deluge web ui.

9. Setup all the different containers
      - Deluge needs to be setup to download to `/downloads/incomplete` and move completed downloads to `/downloads/completed`
      - You need to add some torrent trackers to Jackett
      - For sonarr and radarr you need to connect them to deluge and to the trackers you set up on Jackett. You may also need to click add movie/add series and setup the path to be `/movies` and `/tv` respectively.
      - Ombi needs to be connected to sonarr, radarr and jellyfin. You could also set up passwordless login if you're only going to be using it on your network
      - IIRC you just need to go through the setup wizard for Jellyfin
      - Add links to everything on Heimdall
   
10. install jellyfin locally separated form the docker.