# Local Feral Services
A repo for using OVPN from Feral, but running Radarr / Sonarr etc. on a local server.

**NOTE:** this is applicable on x86 Linux hosts

## Initial Configuration
On your local host, in your chosen directory...let's assume it is "/share/Containers"

Copy the ".env" & "docker-compose.yaml" files from this repo into it.

Edit the .env file, change the values to whatever is needed.
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

## Volume mapping
You need to install the vieux docker plugin (https://github.com/vieux/docker-volume-sshfs) to be able to use the remote drive mount.

Firstly, ensure you have an ssh public key added to your Feral host's ~/.ssh/authorized_keys file.

Then, run this (providing the local path to the .ssh directory where your key is stored):
```
docker plugin install vieux/sshfs sshkey.source=/path/to/local/.ssh/
```

## Start up
Next step, in the chosen directory ("/share/Containers") run:
```
docker-compose up -d
```

## Logging and checks
You can now run "docker-compose logs gluetun" to check the status of the VPN connection and look for any errors.
```
docker-compose logs gluetun
```

You can do the same for the other services ("radarr" / "sonarr" / "lidarr" / "jackett" / "autobrr")

You can also "sh" into the containers by running:
docker-compose exec {service} sh

e.g.
```
docker-compose exec radarr sh
```

Then you can navigate inside the container by "cd" or "ls" etc.

## Configuration of services
Within the Radarr / Sonarr etc. services, you would add your Feral download client, but add the remote path mapping as well.
This means that your local services can then download / copy / import using the locally mounted path (/mnt/feralMedia/movies/ for example)
![image](https://github.com/GitHubMilo/LocalFeralServices/assets/22710842/ac86a617-0b69-4470-9307-7265f0bfc41f)

In your "Indexer" setup, you can add the local "Jackett" instance (copy the "Torznab" feed from within Jackett):
![image](https://github.com/GitHubMilo/LocalFeralServices/assets/22710842/a51b8275-0cd4-46e9-bb4d-4920ad553e85)
