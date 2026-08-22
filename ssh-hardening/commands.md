# Task 2 - key pair + key login
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
/bin/start-ssh.sh                     
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@10.10.1.30

# Task 3 - student key login
ssh-copy-id -i ~/.ssh/id_ed25519.pub student@10.10.1.30

# Task 4 - hardening 
PermitRootLogin no
PasswordAuthentication no
AllowUsers student
MaxAuthTries 3
kill -HUP $(pgrep sshd)

# Verification
ssh student@10.10.1.30 - succeeds
ssh -o PubkeyAuthentication=no student@10.10.1.30 - refused
ssh root@10.10.1.30 - refused

# Task 5 - fail2ban 
syslogd
# /etc/fail2ban/jail.local:
[sshd]
enabled = true
maxretry = 3
findtime = 600
bantime = 600
fail2ban-client start
fail2ban-client status sshd

# Trigger from Bastion
for user in alice bob carol dave; do ssh -o StrictHostKeyChecking=accept-new -o ConnectTimeout=5 $user@10.10.1.30; done

# Task 6 - SSH tunnel
# On Bastion:
/bin/start-ssh.sh
# edited /etc/ssh/sshd_config: AllowTcpForwarding yes
kill -HUP $(pgrep sshd)

# On Admin:
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@10.10.1.20

# On Internal:
mkdir -p /var/www
echo "<h1>Internal Server</h1>" > /var/www/index.html
cd /var/www
python3 -m http.server 8080 -d /var/www

# Packet captures started on Admin-switch and Internal-switch links

# On Admin - open tunnel and test:
ssh -f -N -L 9090:10.10.1.40:8080 root@10.10.1.20
pgrep -f 9090
curl http://localhost:9090/

# Stop tunnel:
pkill -f 9090
