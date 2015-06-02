# HAProxy BOSH Release Production Configuration & Delpoyment

We will walk through an example of using with Cloud Foundry.

Be sure to first target your BOSH Director:
```sh
bosh target $BOSH_HOST
```

If you are using bosh-lite you target like so:
```sh
bosh target 192.168.50.4 lite
```

Now clone this release and cd into the directory:
```sh
git clone https://.../haproxy-boshrelease.git
cd haproxy-boshrelease
```

If you intend on using a final release upload it like so:
```sh
bosh upload release releases/haproxy-1.yml
```

Next download your manifest file for the deployment targeted so we can edit it and add the release.

```sh
mkdir -p ~/workspace/manifests
bosh download manifest haproxy-development ~/workspace/manifests/haproxy.yml
bosh deployment ~/workspace/manifests/haproxy.yml
```

Alternatively, you can make your manifest. For example to prepare a manifest for 
bosh-lite (warden) using the 'centos' stemcell we would do the following:

```sh
STEMCELL_OS=centos ./haproxy-dev manifest warden
```

Edit the manifest file you downloaded (`~/workspace/manifests/haproxy.yml`) and add settings as follows.

Add to the list of known `releases: `

```yaml
releases:
#...
- name: haproxy
  version: latest
```

For consistency also add to the `releases: ` section under `meta: `

```yaml
meta:
  environment: haproxy-development
  releases:
  - name: cf
    version: latest
  - name: haproxy
    version: latest
```

Add properties such as tags.

```yaml
properties:
# ... lots of properties ... at bottom put vv
  haproxy:
```

Now, for every `instances: ` entry you wish to collocate this release with under `jobs:` add the following in the `templates: ` section:

```yaml
  - name: haproxy
    release: haproxy
```

Now you can deploy,

```yaml
bosh -n deploy
```

Note that for each job you create in your release that you want to run on a 
Job VM you must add a `templates:` entry with the `name:` of the template
and the `release:` from which it comes.


