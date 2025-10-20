# dokku-tailscale

Make apps available to your tailnet.

## Install

```sh
dokku plugin:install https://github.com/andrew-womeldorf/dokku-tailscale.git tailscale
```

## Prepare

Set an auth key or oauth client key to the global config:

```sh
dokku config:set --global TS_AUTHKEY=tskey-client-00000000000000000-000000000000000000000000000000000
```

&nbsp;

_(Optional)_ Set login server url to the global config:

```sh
dokku config:set --global TS_LOGIN_SERVER=https://headscale.example.com
```

_(Optional)_ Set tailscale extra args to the global config:

```sh
# Example to force re-authentication
dokku config:set --global TS_EXTRA_ARGS="--force-reauth"
```

Presently, all apps will be tagged in the tailnet with `dokku`.

## Usage

Tailscale can expose a container to your tailnet as a machine by running the
tailscale image as a sidecar, and setting your app container's network mode to
share the tailscale container's network. See Tailscale's [docker
documentation](https://tailscale.com/kb/1282/docker) or their [blog
post](https://tailscale.com/blog/docker-tailscale-guide) on the topic.

This plugin manages the sidecar for you, and can optionally serve traffic
straight to the app port with `tailscale serve`.

See the available commands:

```sh
dokku tailscale:help
```

### Examples

#### From Scratch

1. Create an app `dokku apps:create <app>`
2. Run `dokku tailscale:up <app>` on the app
3. Tailscale resources will be removed when you `dokku apps:destroy`

The tailscale machine will take the same name as the app.

For example, to run nginx on port 80:

```sh
dokku apps:create testing-nginx-on-dokku
dokku tailscale:up testing-nginx-on-dokku
dokku git:from-image testing-nginx-on-dokku nginx
```

Visit `http://testing-nginx-on-dokku` from a device on your tailnet. You should
see the default nginx page.

Now destroy the app:

```sh
dokku apps:destroy testing-nginx-on-dokku
```

#### Add to existing container

If you added tailscale to an app which already had a container running in it,
you will need to restart the container so that it joins the tailscale
container's network:

```sh
dokku ps:restart <app>
```

The tailscale plugin doesn't do that automatically, as this may cause downtime
during the restart, and it's better for the caller to decide when and how to do
this.

#### Containers running not on port 80

Create a container which runs on any other port. Here, we'll start a Traefik v2
container with the dashboard/api exposed insecurely on port 8080:

```sh
dokku apps:create testing-traefik-on-dokku
dokku tailscale:up testing-traefik-on-dokku
dokku config:set testing-traefik-on-dokku TRAEFIK_API=true TRAEFIK_API_INSECURE=true
dokku git:from-image testing-traefik-on-dokku traefik:v2.5
```

Test by visiting http://testing-traefik-on-dokku:8080 in your browser.

To remove the need for appending `:8080` to the end of the url, you can use
`tailscale serve`, which will also add a Let's Encrypt certificate and serve
traffic over https:

```
dokku tailscale:serve testing-traefik-on-dokku 8080
```

#### Exposing to the public internet with Funnel

If you want to make your app accessible from the public internet (not just your tailnet), you can use `tailscale funnel`. **Note: You must first run `tailscale:up` to enable tailscale for the app.**

```sh
# First, enable tailscale for the app
dokku tailscale:up testing-traefik-on-dokku

# Then enable funnel to expose it publicly
dokku tailscale:funnel testing-traefik-on-dokku 8080
```

This will:
- Enable Tailscale Funnel for your app
- Expose the specified port to the public internet via HTTPS
- Display the public funnel URL that anyone can access

**Important notes about Funnel:**
- Requires Tailscale v1.38.3 or later
- Requires MagicDNS and HTTPS certificates enabled for your tailnet  
- Only works on ports 443, 8443, and 10000
- Traffic is subject to bandwidth limits
- The tailscale container must remain running to keep the funnel active

To stop the funnel:

```sh
dokku tailscale:funnel testing-traefik-on-dokku stop
```
