# spectre
https://pst.host as a public secure pastebin service

You need to make sure to clone and checkout the 1-stable branch then run the following:

1. Run `go get` from the Ghostbin directory.
2. Run `go build`.
3. Setup nginx virtualhost file:


Create a file at /etc/nginx/sites-available/ called ghostbin, and paste the following content:

```
# Upstream configuration
upstream ghostbin_upstream {
     server ADDRESS:PORT;
     keepalive 64;
 }


# Public
server {
    listen 80;
    server_name ghostbin.YOURDOMAIN.com; # domain of my site

   location / {
        proxy_http_version 1.1;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header   X-NginX-Proxy    true;
        proxy_set_header   Host             $http_host;
        proxy_set_header   Upgrade          $http_upgrade;
        proxy_redirect     off;
        proxy_pass         http://ghostbin_upstream;
    }
}
```
4. you should be able to run `./ghostbin` in your Ghostbin directory now

If you have issues, refer to [this guide](https://www.dexa-dev.com/hosting-your-own-ghostbin/) and the [wiki](https://github.com/DHowett/spectre/wiki)
