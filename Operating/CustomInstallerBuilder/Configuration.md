# Configure your install

Now that you have [installed the Custom Installer Builder dependencies and code](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Installation.md), let's configure your instance!

-----


## Create and configure local settings
Back in the `custominstallerbuilder` directory, copy the `local_template` folder to `local` (this is where Django will expect the configuration files to live):

```sh
$ cd ~/custominstallerbuilder   # Go into the repo we checked out during setup
$ cp -R local_template local
```

Edit `local/settings.py` to match your local configuration. This is what is contained in the original file (with omissions):

```python
SECRET_KEY = '***** This should be changed to a random string *****'
...
SERVE_STATIC = False
...
BASE_URL = 'http://domain.com/'
...
PROJECT_URL = BASE_URL + 'custominstallerbuilder/'
```


Change it to reflect your setup:
```python
SECRET_KEY = "Don't dare to think using this as your random string either"
...
SERVE_STATIC = True
...	
BASE_URL = 'http://YOUR-URL:PORT/'
...
PROJECT_URL = BASE_URL
```

Specifically,
* `SECRET_KEY` should be a random string of your own choosing. Do not share it with others!
* If you are testing locally, use `http://127.0.0.1:PORT/` for the `BASE_URL`. You may use your public facing IP if you want it to be accessible via the internet.
* If you are testing your setup using a non-privileged account, `PORT` must be an open port number greater than 1024. Port numbers below that (such as the usual HTTP and HTTPS ports 80 and 443) require administrative privileges (`sudo`).

Save the file, and you are done with configuring Django for your Custom Installer Builder. Good job!

## Adapt the build scripts
Go to the `~/custominstallerbuilder/` directory. Edit `rebuild_base_installers.sh` to match your local configuration.

Adapt the file names (including full paths) to your softwareupdater's public and private keys. (If you don't have softwareupdater keys yet, see below how to create them)
```sh
PUBLIC_KEY_FILE=/path/to/publickey
PRIVATE_KEY_FILE=/path/to/privatekey
```

Here's how to generate a key pair in the correct format. Change to the `repy_runtime` directory previously generated, and run the `generatekeys.py` script:
```sh
cd ~/repy_runtime
python generatekeys.py /path/to/key_name 4096
```

This generates a keypair consisting of files `key_name.publickey` and `key_name.privatekey` in the path specified. 4096 is the minimum recommendable length at the time of writing. It is a good idea to make `root` the owner and group-owner of these files, and move them into the `root` account's home dir, `/root`.


Back in `rebuild_base_installers.sh`, remember to update the path-to and name of keyfiles.

Furthermore, the softwareupdater URL must be given:
```sh
SOFTWARE_UPDATE_URL=http://blackbox.poly.edu/updatesite/
```

Also, change
```sh
REPO_PARENT_DIR=/home/cib/custominstallerbuilder/DEPENDENCIES/
```
to the location of your `DEPENDENCIES` folder.

You can also change 
```sh
BASE_INSTALLER_DIRECTORY=/var/www/dist
BASE_INSTALLER_ARCHIVE_DIR=/var/www/dist/old_base_installers
```

to where you would like the base installers located. Just **make sure that the paths exist and that your `cib` user has write access**!

Also change the user name to your Custom Installer Builder user:
```sh
USER=cib
```

## Everything set up and configured!

Continue now to [making the last changes and building installers...](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/CustomizationAndBuild.md)