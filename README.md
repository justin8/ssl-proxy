# ssl-proxy-stack

This stack is used to spin up an nginx container that automatically generates new SSL certs and vhosts with just a few settings.

## Usage
Just start this stack, and then start other containers that you want to host on this machine. Everything else is automatic!

### Installation
1. Run `./install` to install and start the service

### To add another endpoint:
Make sure the domain name you intend to use is already redirecting to this stack on the internet (for let's encrypt to be able to generate valid certificates). Then start a container with the following:

* Expose the port you want to proxy to
* Set the following environment variables on the container:
    * `VIRTUAL_HOST=foo.com` - This supports multiple domains, just seperate them by commas in this variable
    * `VIRTUAL_PORT=1234` - The port of the service you are running, the same as the one that is being exposed
    * `LETSENCRYPT_HOST=foo.com` - This should be identical to `VIRTUAL_HOST` in most circumstances. Any hosts listed in `VIRTUAL_HOST` but not in this key will be HTTP only.
    * `LETSENCRYPT_EMAIL=bar@foo.com` - This must be at the same or a higher level domain than the one you're requesting a certificate for

## Testing
It's possible to use the Let's Encrypt staging servers to make sure your setup is working as expected; it doesn't have the activation limits that the real one does. Certs won't be truly valid, but everything else is exercised by this. Just start new containers with `LETSENCRYPT_TEST=true` and the staging server will be used instead.
