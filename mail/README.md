##

Docker Smarthost / Relay Image
Originally branched from https://github.com/BytemarkHosting/docker-smtp

## Quick reference

This image allows linked containers to send outgoing email. You can configure
it to send email directly to recipients, or to act as a smart host that relays
mail to an intermediate server (eg, GMail, SendGrid).

## Usage

### Basic SMTP server

In this example, linked containers can connect to hostname `mail` and port `25`
to send outgoing email. The SMTP container sends email out directly.

```
docker run --restart always --name mail -d bytemark/smtp

```

### SMTP smart host

In this example, linked containers can connect to hostname `mail` and port `25`
to send outgoing email. The SMTP container acts as a smart host and relays mail
to an intermediate server server (eg, GMail, SendGrid).

```
docker run --restart always --name mail \
    -e RELAY_HOST=smtp.example.com \
    -e RELAY_PORT=587 \
    -e RELAY_USERNAME=alice@example.com \
    -e RELAY_PASSWORD=secretpassword \
    -d bytemark/smtp

```

### Environment variables

All environment variables are optional. But if you specify a `RELAY_HOST`, then
you'll want to also specify the port, username and password otherwise it's
unlikely to work!

* **`MAILNAME`**: Sets Exim's `primary_hostname`, which defaults to the
  hostname of the server.
* **`RELAY_HOST`**: The remote SMTP server address to use.
* **`RELAY_PORT`**: The remote SMTP server port to use.
* **`RELAY_USERNAME`**: The username to authenticate with the remote SMTP
  server.
* **`RELAY_PASSWORD`**: The password to authenticate with the remote SMTP
  server.
* **`RELAY_NETS`**: Declare which networks can use the smart host, separated by
  semi-colons. By default, this is set to
  `10.0.0.0/8;172.16.0.0/12;192.168.0.0/16`, which provides a thin layer of
  protection in case port 25 is accidentally exposed to the public internet
  (which would make your SMTP container an open relay).