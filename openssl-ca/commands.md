mkdir -p /root/ca/certs /root/ca/crl /root/ca/newcerts /root/ca/private

touch /root/ca/index.txt

echo 1000 > /root/ca/serial

openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out /root/ca/private/root-ca.key

openssl req -x509 -new -key /root/ca/private/root-ca.key -sha256 -days 3650 -config /etc/ssl/openssl.cnf -extensions v3_ca -out /root/ca/certs/root-ca.crt -subj "/C=AU/ST=QLD/O=CQUni/CN=Root CA"

mkdir -p /root/ca/intermediate/certs /root/ca/intermediate/crl /root/ca/intermediate/csr /root/ca/intermediate/newcerts /root/ca/intermediate/private

openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out /root/ca/intermediate/private/intermediate.key

openssl req -new -key /root/ca/intermediate/private/intermediate.key -out /root/ca/intermediate/csr/intermediate.csr -subj "/C=AU/ST=QLD/O=CQUni/CN=Intermediate CA"

openssl x509 -req -in /root/ca/intermediate/csr/intermediate.csr -CA /root/ca/certs/root-ca.crt -CAkey /root/ca/private/root-ca.key -CAcreateserial -out /root/ca/intermediate/certs/intermediate.crt -days 1825 -sha256 -extensions v3_ca -extfile /etc/ssl/openssl.cnf
