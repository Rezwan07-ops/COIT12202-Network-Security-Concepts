mkdir -p /root/ca/certs /root/ca/crl /root/ca/newcerts /root/ca/private

touch /root/ca/index.txt

echo 1000 > /root/ca/serial

openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out /root/ca/private/root-ca.key

openssl req -x509 -new -key /root/ca/private/root-ca.key -sha256 -days 3650 -config /etc/ssl/openssl.cnf -extensions v3_ca -out /root/ca/certs/root-ca.crt -subj "/C=AU/ST=QLD/O=CQUni/CN=Root CA"
