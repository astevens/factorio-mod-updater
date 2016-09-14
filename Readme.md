
This will automatically update all mods in a Factorio installation. It is intended to be used on a headless server on Linux and run by a cron job, but works on any distro of Factorio.

After using the very nice [factorio-updater](https://github.com/narc0tiq/factorio-updater) script I found that my local Factorio started to have a lot of mod updates that the server didn't. Copying them manually is a bit of a hassle. Remembering to check everyday would require me to be sober occasionally. Who's got time for that?

## Use ##
You'll need ruby installed. Any reasonably recent version will do nicely.

### Ubuntu/Debian ###
```
sudo apt install ruby -y
```

### Fedora/CentOS ###
```
sudo yum install ruby -y
```

Get a copy of this script on your server:
```
sudo mkdir /opt/factorio-mod-updater
sudo chown `whoami` /opt/factorio-mod-updater
git clone https://github.com/astevens/factorio-mod-updater /opt/factorio-mod-updater
```

This was deliberately written to use no dependencies or libraries so that it can just be copied to your server.

---

### Config ###
If you setup your server to math the defaults in [factorio-init](https://github.com/Bisa/factorio-init) (in /opt/factorio) then you're almost done!

You probably already have your username and token configured in your server-settings.json.

Make sure you have a mods directory:
```
mkdir -p /opt/factorio/mods
```

And copy a mod-list.json in there (macOS):
```
scp ~/Library/Application\ Support/factorio/mods/mod-list.json factorio@supersexyserver:/opt/factorio/mods/
```

On Windows it's location is:
```
%APPDATA%/Factorio/mods/mod-list.json
```

### Run it ###
Just execute it to use the default paths and user config:
```
/opt/factorio-mod-updater/factorio-mod-updater
```

and you will see it check your mods:
```
Checking AddAssemblerBatteries, bobtechsave, Power Armor MK3, ZRecycling
Connecting to mods.factorio.com
AddAssemblerBatteries by DRY411S 0.14.2
    Current
bobtechsave by Bobingabout 0.14.1
    Current
Power Armor MK3 by jimmy_1283 0.0.4
    Current
ZRecycling by DRY411S 0.14.1
```

Mods do not need to be installed or any particular version. The latest version of anything in your mod-list.json will be installed and any old versions will be removed.

---

If you play Factorio you probably like automation. Let's assume you have [factorio-init](https://github.com/Bisa/factorio-init) and [factorio-update](https://github.com/narc0tiq/factorio-updater) installed and configured...
```
crontab -e
```

```
0 10 * * * /opt/factorio-init/factorio stop; /opt/factorio-init/factorio update; /opt/factorio-mod-updater/factorio-mod-updater; /opt/factorio-init/factorio start
```

This will stop your server, update the factorio version, update all mods, and start your server at 10:00 am - my server is running in GMT, so this is early in the morning local time.

Now sit back and enjoy. Fresh mods make you 16% more attractive to the opposite sex.

---

### Detailed config ###
Paths and user details can be overridden if needed.

```
Usage: factorio-mod-updater [options]
    -s, --server-settings PATH       location of server-settings.json for username and token
    -u, --username USERNAME          factorio.com username instead of server-settings.json
    -t, --token TOKEN                factorio.com token instead of server-settings.json
        --mod-host SERVER            server to query for mod data and downloads
    -p, --path PATH                  directory to Factorio
    -m, --mods-path PATH             location of mods dir containing mod-list.json
    -h, --help                       Prints this help
```

If you have a different factorio location or special name for your server-config you can feed it the path:
```
factorio-mod-updater -p /var/srv/factorio -s ~/factorio/happyfunconfig.json
```

server-settings.json is only used for username and token. You can skip it if you specify them on the command line:
```
factorio-mod-updater -u CoolAccountName -t 123GIBBERISH
```

---

### Future ideas ###
- install and remove mods
- match mods and versions of remote server
- some kind of integration with factorio-init scripts
- save settings to a config file?
- use a mod/auth config from a remote url?
- figure out how to scp/echo mod-list from Windows easily

---

Pull requests and bug reports are always welcome!

Thanks to [factorio-init](https://github.com/Bisa/factorio-init) and [factorio-update](https://github.com/narc0tiq/factorio-updater)!
