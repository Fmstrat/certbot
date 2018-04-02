
Setting up your DNS
-------------------

This assumes:

- Your domain is `example.com`
- The host you run Easywildcard on is `mydynamichost.example.com`
- `mydynamichost.example.com` uses Dynamic DNS to stay updated

To get things going, create the following DNS records:

- `acme-dns.example.com` that is a `CNAME` to `mydymanichost.example.com` (or an `A` record to a static IP)
- `acme-challenge.example.com` that is an `NS` type with an answer of `acme-dns.exmaple.com`
- `_acme-challenge.example.com` that is a `CNAME` to `acme-challenge.exmple.com`

That's it!


Setting up the machine to run Easywildcard
------------------------------------------

- Make sure your Dynamic DNS provider is set up so your IP always has a static hostname (or use a provider with a static IP)
- Make sure you forward port `53` externally to port `5053` on the local machine (or whatever port is specified below)


Usage
------------

Until this is live on the HUB and documentation is complete, clone and then:

  docker build --tag easywildcard .
  DOMAIN="example.com"
  EMAIL="letsencrypt@example.com"
  PORT=5053
  mkdir -p /tmp/letsencrypt

Then, to get a new cert:

  docker run -p ${PORT}:53/tcp -p ${PORT}:53/udp -v /tmp/letsencrypt/:/etc/letsencrypt --rm -i -e "EMAIL=${EMAIL}" -e "DOMAIN=${DOMAIN}" easywildcard

You can run the above for as many domains as you need SSL certs for.

From then out, to renew:

  docker run -p ${PORT}:53/tcp -p ${PORT}:53/udp -v /tmp/letsencrypt/:/etc/letsencrypt --rm -i -e -e "RENEW=1" easywildcard

You can add certs at any time using the first command. Renew will just pick them up
