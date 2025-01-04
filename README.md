# Reverse Proxy

## Basic NGINX config
To get started, create a nginx configuration file in `/etc/nginx/sites-available` called `desired-sub.domain.se`. Add configuration from the example given in this repo. I've removed the SSL stuff because it's managed automatically. 

To test the config without actually restarting the nginx server, run `sudo nginx -t`. 

To restart the nginx server, run `sudo systemctl reload nginx`. 

Now you should be able to reach the desired application at that specific subdomain. 

## Firewall

There might also be some things you want to configure with the firewall. This can be done with a utility program `ufw` (uncomplicated firewall).

This might first need to be enabled using `sudo ufw enable`, and then you want to add specific rules to allow this nginx instance to receive traffic  `sudo ufw allow 'Nginx Full`. Apart from this, after enabling the firewall, you probably also want to run `sudo ufw allow ssh`, to not stop yourself from being able to reconnect using SSH.

## HTTPS / SSL

Install `certbot` and `python3-certbot-plugin` using apt or similar (`sudo apt install certbot python3-certbot-plugin`).

Run `sudo certbot --nginx -d desired-sub.domain.se`. 

Answer a few questions, and then you should be all good. The python plugin will make all necessary configuration changes for the subdomains. 

## Lessons learned 

I learned that it can be tricky to try to have some service on just a particular route on the same subdomain. Doing this means first that you have to manually add it again after having routed to it, in the config. But that's not the tricky part, the tricky part is making sure that your application is fine with having that initial prefix. On a nuxt application, that was fine because it's possible to set like a baseUrl prefix. But for homeassistant, that proved more difficult so I ended up just creating different subdomains. I was a little concerned about HTTPS, but it turns out it's really simple to get up and running using certbot. 
