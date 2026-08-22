mkpasswd -m md5crypt pass12286418
mkpasswd -m sha512crypt pass12286418
mkpasswd -m yescrypt pass12286418

useradd -p '$1$9etMIV6a$yFb0o2UGcznG/nqKDGYRi.' user_md5
useradd -p '$6$WTKPiJGb6iQDRgaI$Crk13OCUobTQEqhnAPxCLbRBNCEzBBgsuvVwc0HzQNTQ6PriV2/qb0taIO/ElEgjVvEAib57cAbv6zu2GCu5X/' user_sha512
useradd -p '$y$j9T$DLJVmzxkbVGQjXytGUz1G.$cuq/nPBxNA4X/FPvW3/7rlataAxoa3Dydj6MItxQfH6' user_yescrypt

cat /etc/shadow | grep user_

password requisite pam_pwquality.so retry=3 minlen=12 ucredit=-1 dcredit=-1 enforce_for_root dictcheck=0
useradd -m user_test
passwd user_test

cp /etc/pam.d/common-auth{,.bak}
# /etc/security/faillock.conf additions:
deny = 5
unlock_time = 300

auth    required                        pam_faillock.so preauth
auth    [success=1 default=ignore]      pam_unix.so nullok
auth    [default=die]                   pam_faillock.so authfail
auth    sufficient                      pam_faillock.so authsucc

su - user_test
su - user_test   # wrong password x5 to trigger lockout
faillock --user user_test
faillock --user user_test --reset

grep -x 'password' /usr/share/wordlists/rockyou.txt
useradd -p "$(mkpasswd -m md5crypt password)"     crack_md5
useradd -p "$(mkpasswd -m sha512crypt password)"  crack_sha512
useradd -p "$(mkpasswd -m yescrypt password)"     crack_yescrypt

grep '^crack_' /etc/shadow > hashes.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
john --format=crypt --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
john --show --format=crypt hashes.txt
