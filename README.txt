DEPRECATED
    This project is no longer maintained. No further updates, bug fixes, or
    feature additions will be made. Use at your own risk.

dyndns updater
==============

You need a running BIND on a server box with root access.
You also need a new zone for your dyndns host.

1) Add new user dyndns to your DNS server box.
2) Read the nsupdate manpage at find out how to use it. 
   Create a nsupdate key called your-host-name if your host
   is your.host.name. 
3) Copy the nsupdate key files of your host to ~dyndns and
   chown them to the dyndns user.
4) Also read the ssh manpage because you need to gerenate 
   a RSA Public Key if you want to update a dyndns host via
   SSH without an interactive password prompt.
5) Create an empty /var/log/lastnsupdate.log file and chown
   it to the user dyndns.

Further explanation:

Call this script from a DynDNS-client box:
Syntax: 
  ssh dyndns@dyndnsserver /path/to/dyndns-update \
  your.host.name. TYPE new-entry TIMEOUT
An real example: 
  ssh dyndns@dyndnsserver /path/to/dyndns-update \
  local.buetow.org. A 137.226.50.91 30

Because its a dyndns host, we set the timeout to very low
30 seconds.

If you use ppp on your client box, then you may add the 
following lines to your /etc/ppp/ppp.linkup file:

 ! sh -c "echo \"MYADDR `date`\" >> /var/log/ips.log"
 !bg /usr/bin/ssh dyndns@dyndnsserver \
  /path/to/update-dyndns your.host.name. A MYADDR 30

Here you need to generate a SSH RSA public key to allow root
to ssh into dyndns@dyndnsserver without a password. Ask 
google or read the manpage and try it out! RTFM!

your.host.name's A record should be changed to the dyndns 
client's new ip address each time the internet protocol 
number changes.
You may use the tools nslookup and dig to check if its working.

Thanks to Lars Engels for some nice ideas concerning 
ppp.linkup and nsupdate! :).
