# Warden — deploy bundle

Thin installer for **Warden** (self-hosted FortiGate change-tracking + CIS
compliance). This public repo holds **no application code and no secrets** — just
the compose file and install scripts. The app itself ships as **private**
container images that require your registry login to pull.

## Install on a fresh Ubuntu 22.04 / 24.04 box

```bash
sudo mkdir -p /opt/warden
wget -qO- https://raw.githubusercontent.com/jciso/warden-deploy/main/warden-deploy.tar.gz \
  | sudo tar -xz -C /opt/warden
cd /opt/warden && sudo bash scripts/install-prod.sh
```

The installer will:
- install Docker if it's missing,
- ask for the registry/tag (press Enter for defaults),
- log you in to the registry (**paste your read-only pull token** at the prompt),
- generate this box's own unique secrets,
- pull the images and start the app,
- install the on-demand self-updater.

Then open **`http://<box-ip>:8080`** and create your admin in the setup wizard.

> Back up `/opt/warden/.env` — it holds this box's `FWTRACK_ENC_KEY`, and a
> database backup cannot be restored without it.

## Updating later

An admin clicks **Software update → Update now** in the app (or runs
`warden-update` on the box). Updates are pulled on demand; nothing changes until
a new version is published **and** someone applies it.
