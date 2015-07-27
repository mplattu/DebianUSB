# DebianUSB for Abitti network

This Finnish document explains how to install Debian 8.1 to USB and make 
it a base for an Abitti (http://abitti.fi) server replacement. Installation
steps:

1. Install Debian server to a USB memory stick, USB hard disk or even to
the hard disk of your workstation by following instructions in `INSTALL.md`.

2. Install your favourite test suite (e.g. Moodle, TAO) to this server. 
Make sure your server listens to port 443. It does not have to use SSL 
although this is of course a good idea.

3. Download and deploy Abitti client image using 
[AbittiUSB](http://www.abitti.fi/fi/ohjeet/usb-tikkujen-kopiointi/).

4. After your students have booted the client images browse to
* `https://koe`
* `http://koe:443` (in case your server does not have SSL enabled)

# No Support or Warranty

*This documentation is not supported by Matriculation Examination Board 
of Finland. There is no guarantee that a server will work with further
versions of Abitti client OS.*
