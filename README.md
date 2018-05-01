# unifi
Vagrantfile for Unifi Controller on Debian Stretch

You'll want to change the bridged interface to match whatever interface you want
to expose.

This also is still exposing ssh on port 22 on the bridged interface, which is
unnecessary, however I intend to run the controller only when I'm pushing
changes. 
