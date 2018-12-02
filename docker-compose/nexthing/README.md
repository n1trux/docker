# Nexthing

Combines Nextcloud + Syncthing.

This docker-compose setup will provide you with two web interfaces:

- `https://cloud.example.com` – Nextcloud
- `https://sync.cloud.example.com` – Syncthing.

## Configuration

Edit the base URL in `config/web.env`. You should also set a password in `config/db.env`.

## Installation

Copy all files to a folder of your choice, then go to that folder and execute

    docker-compose up -d

You can check the progress of the `db` and `sync` containers with `docker-compose logs`. If nothing happens, the stack is ready. This takes up to five minutes.

Create an admin user for Nextcloud (URL: see above). The default files and folders as well as the database will be populated, this can also take a few minutes. Do not close the browser window in that timeframe.

If an error occurs, just wait a few minutes and reload the page. You can probably log in with the user you created before.

After setting up the user and logging in for the first time, you can visit the Syncthing web interface (URL: see above).

You should set a password in Settings → GUI → GUI Authentication User/Password. DO NOT check "Use HTTPS for GUI" as that breaks the traefik setup!

All files and folders of Nextcloud will be accessible in Syncthing via the path `/data/USERNAME/files/`. Replace USERNAME with your Nextcloud username. You can then sync these folders with other Syncthing instances.

Keep in mind that you have to log in a single time before you'll be able to select the folders in Syncthing, as Nextcloud still has to create them.

## Add users (Nextcloud)

Go to the Nextcloud web interface. Log in as admin user, click the cogwheel in the top right and select _Users_.

## Add devices (Syncthing)

Click _Add Remote Device_ in the bottom left of the web interface.
