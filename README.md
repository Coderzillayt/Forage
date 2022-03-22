# Forage
A proxy frontend based upon the [Corrosion](https://github.com/titaniumnetwork-dev/Corrosion)
and [Womginx](https://github.com/binary-person/womginx) backends.

## Deployment
These instructions assume you have already obtained domains
and their respective certificates and configured DNS.
They also assume the server is running a Debian-based
system (like Ubuntu).

First, install the necessary programs:
```bash
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash -
sudo apt update
sudo apt install -y nodejs nginx yarn build-essential
```

Then, setup the Forage code:
```bash
git clone https://github.com/funnycorp/Forage
cd Forage
yarn
ln -sf $(pwd)/generated-conf/main.conf /etc/nginx/nginx.conf
```

You may now edit the `config.json` file if you want to
customize the redirects, Womginx sites, and blacklists.

You must modify `domains.json` however.
It is an object, with the key being the domain Forage is
being hosted on, and the value being another object.
The inner object has a `stable` property, indicating if
the domain is known to work (not hindered by web filters,
Safe Browsing, etc.).
The `cert` property denotes the location of the domain's
SSL/TLS certificate, and the `key` property contains the
domain's private key for TLS.

Once you edited `domains.json` accordingly, you can now
start Forage using a program like forever or PM2. Once you
start Forage, you may start nginx using `sudo service nginx
start` or `sudo systemctl start nginx`. **You must start
Forage before nginx on your first run and after any
changes in the nginx configuration in the `conf/`
subdirectory or `domains.json`.** This is due to the
configuration files being generated by Forage as it starts
up.

Once you set up `domains.json` and started Forage and nginx
in the background, Forage should be up and running on your
domains.

## License
Because of Forage's dependencies on Womginx and Wombat, it
is licensed under the AGPL to comply with their licenses.