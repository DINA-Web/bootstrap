# Bootstrapping DINA-Web

The core components of the DINA-Web system include the Collections Management UI (frontend Ember JS client) and the Collections API (backend REST API). These are pinned repositories listed at https://github.com/DINA-Web/. In addition there is a reverse proxy for web traffic routing and SSL termination that is needed to start a minimal DINA-Web system composition.

## Requirements

To build from sources and run the system locally, you need some *nix host with `docker` and `docker-compose`, `make` and `git` installed. If you bootstrap the system for development purposes, starting from scratch, we recommend that you use an up-to-date Linux desktop OS in the continuation. The host where you run the system can then be your laptop or it could be some other networked *nix box that you have access to. 

As a time-saver, for bootstrapping convenience, if you are on a non-FOSS operating system, consider to use VirtualBox and start up for example a recent Linux Mint distro for a smooth experience. Then install `docker` with `docker-compose` (there are great official guides for this - use a search engine) and other required system libraries (`sudo apt install make git`).

To start up this system locally you then need to get several repositories. The README.md files in those repositories gives startup instructions, which often involve using the `Makefile` to work with the project's compositions of components as listed and defined in more detail in the `docker-compose.yml` file.

## Step-by-step instructions

### Services

Here is an attempt to provide a short recipe of commands you can use to get the necessary parts in place.

		# backend with REST API
		git clone https://github.com/DINA-Web/collections-api-docker
		cd collections-api-docker
		git fetch origin
		git checkout --track origin/summer-diff
		make init
		make
		cd ..

		# frontend with Ember JS client
		git clone https://github.com/DINA-Web/collections-ui-docker.git
		cd collections-ui-docker
		make
		cd ..

		# reverse proxy with SSL termination
		git clone https://github.com/DINA-Web/proxy-docker.git
		cd proxy-docker
		git fetch origin
		git checkout --track origin/self-signed-certs
		make

### Other Settings - name resolution and SSL

We suggest you add the following entries to the `/etc/hosts` file so that your host responds to the above services:

		beta-sso.dina-web.net beta-api.dina-web.net beta-cm.dina-web.net

This is not strictly necessary, and name resolution can also be achieved in a more elegant way without "hardcoding" those names in your local /etc/hosts file.  This approach involves using something like `dnsdock`. It involves more steps but will not put entries into `/etc/hosts` that can easily be forgotten afterwards. See https://github.com/mskyttner/dns-test-docker for more details... 

You know you have succeeded in starting the backend services when you can run the `demo.sh` script in the `collections-api-docker` repo with success.

It is then time to setup SSL certs and to launch the Collections Manager UI in the browser. First register the ca.pem certificate with Firefox (or Chrome or equiv) for the self-signed SSL certs to work. Then issue:

		firefox https://beta-cm.dina-web.net

Login using login "johnny" and pass "passw0rd12" and try to register a collection object in the Entomology collection to get some familiarity with the data entry form which is available in Swedish and English.

# Task: Making a minimal change in the Collections Manager UI

If you have succeeded with the earlier steps, it means that you have built the system locally and started it. This means you can now start to change or improve the system.

To make a change in the Collections Manager UI, you also need to get another repository:

		git clone https://github.com/DINA-Web/collections-ui

When you have this repo locally, we ask you to work on this issue:

		https://github.com/DINA-Web/collections-ui/issues/65

## Create a Pull Request

After doing the change above (basically changing a string used in a data entry form label), you need to a rebuild and also make sure that you have your new build running locally. 

When you are happy with the change, you need to give the new build a new version number. Then in the "collections-ui-docker", change the `docker-compose.yml` file to use your freshly built image and verify that the issue is solved.
    
We finally ask you to do a Pull Request with the change at GitHub, ie at https://github.com/DINA-Web.

