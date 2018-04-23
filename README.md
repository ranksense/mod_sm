# mod_ranksense for Apache #
Copyright Ranksense Inc. 2018

## mod_ranksense.c ##

Based on mod_cloudflare.c and mod_remoteip.c, this Apache extension will replace the remote_ip variable in user's logs with the correct remote IP sent from Ranksense. The module only performs the IP substitution for requests containing proper secret key in `X-SM-SECRET` header.

In addition to this, the extension will also set the HTTPS environment variable to "on" in cases where Flexible SSL is in use. This prevents software such as WordPress from being broken by Flexible SSL.

To install, either run apxs2 directly against the .c source file:

    $ apxs2 -a -i -c mod_ranksense.c

An alternative way to install is to use GNU autotools, which requires that autoconf and automake already be installed:

    $ autoconf
    $ ./configure
    $ make
    $ make install
    
### OS Support ###

- CentOS - Supported
- CloudLinux - Not Supported

In order to make module work, please set `RanksenseSecret` value in configuration file, otherwise module will not be active.

### RanksenseRemoteIPHeader ###

This specifies the header which contains the original IP. Default:

    RanksenseRemoteIPHeader X-SM-CONNECTING-IP

### RanksenseSecret ###

If header `X-SM-SECRET` is not set for request, or its value does not match configuration, HTTP status will be 403:

    RanksenseSecret secret-key


## Loading the Module ##

Note that on some systems, you may have to add a `LoadModule` directive manually. This should look like:

    LoadModule ranksense_module /usr/lib/apache2/modules/mod_ranksense.so

Replace `/usr/lib/apache2/modules/mod_ranksense.so` with the path to `mod_ranksense.so` on your system.

## Installing apxs/apxs2 ##

If you cannot find `apxs` or `apxs2`, install `apache2-dev` on Debian and Ubuntu, or `httpd-devel` on Red Hat and CentOS:

    $ apt-get install apache2-dev
    $ yum install httpd-devel

## Additional Notes ##

- If mod\_ranksense and mod\_remoteip are enabled on the same web server, the server will crash if they both try to set the remote IP to a different value.
- Enabling mod\_ranksense will not effect the performance of Apache in any noticeable manner. AB testing both over LAN and WAN show no equivalent numbers with and without mod\_ranksense.

  [1]: https://www.ranksense.com/
