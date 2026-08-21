# COIT12202 - Assessment 1 Hardened Services Portfolio

Individual portfolio for Assessment 1, building and hardening three
security services on GNS3: PKI/HTTPS, password security, and SSH.

### Week 2 - OpenSSL PKI 

A two-tier Certificate Authority is built with OpenSSL and it is used to issue a certificate for a website
(`www.12286418.lab`). This is made over HTTPS through Nginx and the full trust
chain is presented and verified on a separate client.

It contains:
- `root-ca.crt`, `intermediate.crt`, `server.crt`, `ca-chain.crt` - These are the certificates 
- `commands.md` — these commands are used to build the CA hierarchy, sign
  the server certificate, build the chain file and verify it
- The screenshots showing `openssl verify` confirming the chain is OK and a successful `curl` HTTPS request

The screenshots are shown below

**Chain verification:**
![openssl verify showing the certificate chain is OK](OpenSSL-CA-12286418-verify.jpg)

**Successful HTTPS request:**
![curl successfully connecting over HTTPS](OpenSSL-CA-12286418-curl.jpg)
