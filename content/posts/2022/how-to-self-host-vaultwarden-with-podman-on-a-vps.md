+++
title = 'How to self-host Vaultwarden with Podman on a VPS'
description = 'Learn how to self-host Vaultwarden to secure and manage your precious passwords'
date = 2022-09-27T16:20:48-03:00
tags = ['Self-hosting', 'Security', 'Privacy', 'Password Manager']
draft = false
+++

I would say that one of the most important things in our digital life is to have good passwords, then to have a proper way to secure and manage those passwords. Thankfully, we have different solutions.

Today I'm going to show you how to self-host Vaultwarden with Podman[^1].

{{< url "Vaultwarden" "https://github.com/dani-garcia/vaultwarden" >}} is described by its creator as an


> alternative implementation of the Bitwarden server API written in Rust and compatible with upstream {{< url "Bitwarden clients" "https://bitwarden.com/download/" >}}, perfect for self-hosted deployment where running the official resource-heavy service might not be ideal.


## Considerations

+ It is strongly recommended to set up an SSL certificate and make sure your server's firewall, ssh and any other security measures are already done (which I'm not covering in this post).

+ For the SSL certificate, make sure you have registered a domain name (for instance, example.com) and pointed both A and/or AAAA DNS records to the public IPv4 and/or IPv6 addresses of your VPS.[^2]

If you're setting up locally, let's say on a Raspberry Pi, I wouldn't mess with port forwarding, unless you know what you're doing.<br>I'd instead look into a tool like {{< url "Tailscale" "https://tailscale.com/">}} (which is great, free and will allow you to access your passwords even when you're not at home).
{.warning}

## Requirements

+ A Linux distribution (I'm running on Ubuntu Server 22.04 LTS)
+ [Podman](#install-podman);
+ [Caddy](#install-caddy) (or any other webserver of your choice);
+ [Ports 80/tcp and 443/tcp open](#enabling-firewall-ports).

### Install Podman

``` bash
# Ubuntu 20.10 and newer
sudo apt -y update
sudo apt -y install podman
```

For other distros, or if you'd like to install from source, see the {{< url "podman documentation" "https://podman.io/getting-started/installation">}}.

### Install Caddy

```bash
# Ubuntu 20.10 and newer
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt -y update
sudo apt -y install caddy
```

For other distros, or if you'd like to install from source, see the {{< url "caddy documentation" "https://caddyserver.com/docs/install" >}}.

### Enabling firewall ports

I like to use UFW to manage my firewall rules. If you're running newer versions of Ubuntu, it should be pre-installed. Otherwise, you may search for it in your package manager. Feel free to use any firewall to manage your rules if you feel like it.

Before enabling UFW, make sure to allow ssh access, so you don't get kicked out of your server.
{.info}

```bash
sudo ufw allow <your-ssh-port>/tcp
sudo ufw allow 80,443/tcp
sudo ufw enable
```

## Install Vaultwarden

By default, root-less podman containers won't start at boot or keep started when a user is logged out. To allow it to run in the background:

```bash
sudo loginctl enable-linger <your-username>
```

Now, we need to create a volume to mount our container, so we can make sure our data remains permanent after we restart our container.

```bash
podman volume create vaultwarden
```

Let's create a podman container for Vaultwarden[^3]:

```bash
podman run --detach --name vaultwarden --volume vaultwarden:/data/ --restart always --memory 256m --publish 8080:80 --publish 3012:3012 --env WEBSOCKET_ENABLED=true docker.io/vaultwarden/server:latest
```

By now, you should have your container up and running. You can verify that by running `podman ps`.


```bash
CONTAINER ID  IMAGE                                COMMAND            CREATED      STATUS             PORTS                                         NAMES
15e928a3d92e  docker.io/vaultwarden/server:latest  /start.sh          2 weeks ago  Up 43 seconds ago  0.0.0.0:8080->80/tcp, 0.0.0.0:3012->3012/tcp  vaultwarden
```

## Set up Caddy as Reverse Proxy


Open the file **/etc/caddy/Caddyfile**

``` bash
sudo nano /etc/caddy/Caddyfile
```

Comment out all existing lines or just delete them, then add the following lines:


``` toml
example.com {
        reverse_proxy localhost:8080
        reverse_proxy /notifications/hub localhost:3012
    reverse_proxy /notifications/hub/negotiate localhost:8080

        tls <your-email> {
                protocols tls1.3
        }
        header {
                # enable HSTS
                Strict-Transport-Security max-age=31536000;
        }
        encode gzip
}
```

Make sure to edit the first line with your own domain.tld, and the sixth line with the email you'd like to use when generating your SSL certificate.<br>Caddy will take care of the certificate automatically for you.
{.info}

Reload the caddy service:

```bash
sudo systemctl reload caddy
```

Now you should be able to access your Vaultwarden container in your browser by accessing `https://your-domain.tld`.

## Initial Setup

- Navigate to your domain;
- Create an account using a strong password;
- Now login with your credentials and *voila*... you're done!

## Additional Steps

* [Configure Systemd](#configure-systemd), so you can have your container start at boot;
* [Update your container](#update);
* [Head to the official documentation](#official-documentation), so you can configure any additional settings.

### Configure systemd

Navigate to the `~/.config/systemd/<your-user` folder.

`cd ~/.config/systemd/<your-user>`

If the folder does not exist, just create it it<br>`mkdir -p ~/.config/systemd/<your-user>`
{.tip}


Now, you have to create a systemd unit file that can be used to control the container. Luckily podman can help us by issuing the following command:

```bash
# Generate systemd unit file
podman generate systemd --files --name vaultwarden

# Enabling our file to start at boot
systemctl --user enable container-vaultwarden.service
```

The "--user" is a flag, and you should not replace it with your real user.
{.info}

Reboot your server, then `podman ps` to confirm that the container started without any problems.

## Update

The updating process basically consists of pulling a new image, removing the old one, then recreating our container.

```bash
# Check if there's any update for your container image
podman pull docker.io/vaultwarden/server:latest

# If any update, you should stop and delete your current container
podman stop vaultwarden && podman rm vaultwarden

# Now we can recreate our Vaultwarden container
podman run --detach --name vaultwarden --volume vaultwarden:/data/ --restart always --memory 256m --publish 8080:80 --publish 3012:3012 --env WEBSOCKET_ENABLED=true docker.io/vaultwarden/server:latest

# Check if it's up and running
podman ps
```

Feel free to automatize this process if you would like to.

## Official Documentation

{{< url "https://github.com/dani-garcia/vaultwarden/wiki" "https://github.com/dani-garcia/vaultwarden/wiki" >}}

The official Wiki page provides lots of useful information and specific additional configurations you may be interested in like:

* Disable registration of new users;
* Disable invitations;
* Fail2Ban Setup (Recommended if your server is exposed to the internet);
* SMTP Configuration.

## Using Vaultwarden

The Vaultwarden container we just created is compatible with upstream{{< url "Bitwarden clients" "https://bitwarden.com/download/" >}}.

Remember to edit the settings of the application before login. You must point to your custom Server URL.

If you need additional help, check out the {{< url "Bitwarden Help Center" "https://bitwarden.com/help/" >}}.

## Additional Tip

If you want to learn about password manager[^4] alternatives, for instance local-based password managers, the page bellow is great:

{{< url "https://www.privacyguides.org/passwords/" "https://www.privacyguides.org/passwords/" >}}


[^1]: Why Podman instead of Docker?<br><br>I have run into trouble running docker on low-end hardware (like a Raspberry Pi), while Podman works seamlessly.<br><br>In terms of capabilities, as of today I haven't found any missing features on my use cases.

[^2]: You can try to get a certificate using other methods like {{< url "acme.sh" "https://github.com/acmesh-official/acme.sh" >}}.


[^3]: Flags explained:<br><br>--**detach** - daemonizes the container to run in the background;<br>--**name** - defines a name for the container;<br>--**volume** - uses the persisted volume vaultwarden created in the step before;<br>--**restart** - this flag ensures that our container will try to stay up in case of something goes wrong;<br>--**memory** - limits to total amount of memory used by the container (you can definitely change this value as needed or even ignore this flag;<br>--**publish** - defines which host ports will be exposed to the container ports respectively:<br>--**env WEBSOCKET_ENABLED=true** environment variable enables the websocket server for our container.

[^4]:Password managers basically allow you to securely store and manage passwords and other credentials.<br><br>A **cloud-based** password manager allows you to sync all your passwords on a cloud server for easy accessibility and safety against device loss, as I'm going to demonstrate in this post.<br><br>A **local-based** password manager allows you to manage an encrypted password database locally.