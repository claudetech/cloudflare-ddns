# cloudflare-ddns

Simple bash script to keep Cloudflare A record up to date with your current global ip.

Useful for servers without a static ip.

## Dependencies

You will need [jq](https://stedolan.github.io/jq/) and [curl](http://curl.haxx.se/) to run the script.

On Debian/Ubuntu:

```
apt-get install -y curl jq
```


## Usage

Put `ddns` somewhere (on your path if you want) and make it executable.

```
curl -o /path/to/ddns https://raw.githubusercontent.com/claudetech/cloudflare-ddns/master/ddns
chmod +x /path/to/ddns
```

Create a file with the `EMAIL` and `TOKEN` variables containing your Cloudflare email and API key. The default path is `/etc/cloudflare/default` but you can override it with the `DNS_ENV` variable.

You can also set the `ZONE` and `DOMAIN` variables for the A record you want to keep up to date, or pass the variables directly when executing the script.

You can find an example config in [dns_env.example](./dns_env.example).

Do not forget to set the permission to `600` on the file, to avoid having your
Cloudflare API key read.

You can then run the script with:

```
DNS_ENV=/path/to/config ZONE=example.com DOMAIN=foo.example.com /path/to/ddns
```

You can omit `ZONE` and `DOMAIN` if you wrote them in the config file.

Note that it will not work if `DOMAIN` does not have an A record associated.

### Running periodically

To keep your A record up to date, you can add the task to your crontab. After running `crontab -e`, add the following.

```
* * * * * DNS_ENV=/path/to/config /path/to/ddns 2>&1 >> /path/to/logfile
```

This will execute the script every minute.