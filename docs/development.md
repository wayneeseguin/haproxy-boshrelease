# HAProxy BOSH Release Development

Download your manifest file for the deployment targeted.

Target your BOSH Director as explained above, if you already have a deploy be sure to also download your manifest as per above.

If this is your first time cloning the release repository for develompent first prepare all of the things:
```sh
./haproxy-dev all warden
```

This is equivalent to:
```sh
./haproxy-dev blobs
./haproxy-dev release
./haproxy-dev stemcell warden
./haproxy-dev manifest warden
```

Once these steps have all been completed

```sh
bosh -n deploy
```

## Deployments Blobs

The script `./haproxy-dev blobs` is used to prepare the `blobs/` directory
runs each package's `prepare` script from within the `blobs/`
directory. This will run each package's `prepare` script, if it exists,
which *should* download and prepare that package's required blobs 
(source tarballs, etc...) into the {package}/ directory.


## Debugging & QA

See `docs/notes.md` for more information.

In order to gain access to one of the VMs, from a terminal `bosh ssh haproxy {VM Index}`, 
for example to ssh to the first node:
```sh
bosh ssh haproxy 0 # Hop on and have a look around...
```

If you want to destroy and recreate on specific node, say `haproxy/0`, you do the following:

```sh
bosh -n recreate haproxy 0 --force
```

See `docs/haproxy.md` for release specific information.

