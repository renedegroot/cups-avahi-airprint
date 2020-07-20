# renedegroot/cups-avahi-airprint

Fork from [chuckcharlie/docker-cups-airprint](https://github.com/chuckcharlie/docker-cups-airprint)

This Alpine-based Docker image runs a CUPS instance that is meant as an AirPrint relay for printers that are already on the network but not AirPrint capable. The other images were out of date, and seemed not to be working anymore. I forked Alpine image and updated it to work with Python 3. Since I use a HP printer i've included the hplip printing and imaging library.

## Configuration

### Volumes:
* `/config`: where the persistent printer configs will be stored
* `/services`: where the Avahi service files will be generated

### Variables:
* `CUPSADMIN`: the CUPS admin user you want created
* `CUPSPASSWORD`: the password for the CUPS admin user

### Ports/Network:
* Must be run on host network. This is required to support multicasting which is needed for Airprint.

### Example run command:
```
docker run --name cups --restart unless-stopped  --net host\
  -v <your services dir>:/services \
  -v <your config dir>:/config \
  -e CUPSADMIN="<username>" \
  -e CUPSPASSWORD="<password>" \
  chuckcharlie/cups-avahi-airprint:latest
```

## Add and set up printer:
* CUPS will be configurable at http://[host ip]:631 using the CUPSADMIN/CUPSPASSWORD.
* Make sure you select `Share This Printer` when configuring the printer in CUPS.
* ***After configuring your printer, you need to close the web browser for at least 60 seconds. CUPS will not write the config files until it detects the connection is closed for as long as a minute.***

