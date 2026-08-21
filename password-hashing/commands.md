mkpasswd -m md5crypt pass12286418
mkpasswd -m sha512crypt pass12286418
mkpasswd -m yescrypt pass12286418

useradd -p '$1$9etMIV6a$yFb0o2UGcznG/nqKDGYRi.' user_md5
useradd -p '$6$WTKPiJGb6iQDRgaI$Crk13OCUobTQEqhnAPxCLbRBNCEzBBgsuvVwc0HzQNTQ6PriV2/qb0taIO/ElEgjVvEAib57cAbv6zu2GCu5X/' user_sha512
useradd -p '$y$j9T$DLJVmzxkbVGQjXytGUz1G.$cuq/nPBxNA4X/FPvW3/7rlataAxoa3Dydj6MItxQfH6' user_yescrypt

cat /etc/shadow | grep user_
