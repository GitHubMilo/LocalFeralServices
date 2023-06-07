# LocalFeralServices
A repo for using OVPN from Feral, but running Radarr / Sonarr etc.

On your local host, in your chosen directory...let's assume it is "/share/Containers"

Copy the ".env" file from this repo into it.
Edit it, change the values to whatever is needed.
* "NETWORK_SUBMASK" - if your home router has an IP address of 192.168.1.x - then leave it as it is. Essentially, whatever the IP address is, change the last number to a zero and use that.
* "TZ" - this is the timezone, set to a valid TZ identifier (https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
* "PUID" is the local Unix user ID of the user you want the containers to run as.
* "PGID" is the local Unix group ID of the user you want the containers to use.
* "FERAL_USER" - hopefully this is obvious, but it's your Feral unix user account
* "FERAL_HOST" - should be the server you're using.
* "LOCAL_MOUNT_POINT" - if your local music is stored under "/share/Music", then "LOCAL_MOUNT_POINT" is "share"

Next, create a "config" sub-directory ("/share/Containers/config")
In that sub-directory, create one called "vpn" ("/share/Containers/config/vpn")
From your Feral box, get the "~/private/vpn/client.ovpn" file and transfer that to the ./config/vpn directory you just created.

Next step, in the chosen directory ("/share/Containers") run:
```docker-compose up -d```

You can now run "docker-compose logs gluetun" to check the status of the VPN connection and look for any errors.
You can do the same for the other services ("radarr" / "sonarr" / "lidarr" / "jackett" / "autobrr"
You can also "sh" into the containers by running:
docker-compose exec {service} sh

e.g. ```docker-compose exec radarr sh```
Then you can navigate inside the container by "cd" or "ls" etc.
