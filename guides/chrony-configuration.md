# Chrony Configuration

To start go to https://chrony.tuxfamily.org/download.html and download the latest version of chrony.

Then edit the /etc/chrony/chrony.conf file to look like this:

```
# 3 sources per time servers.
pool ntp.ubuntu.com        iburst maxsources 3
pool time.nist.gov         iburst maxsources 3
pool us.pool.ntp.org       iburst maxsources 3

keyfile /etc/chrony/chrony.keys

driftfile /var/lib/chrony/chrony.drift

logdir /var/log/chrony

maxupdateskew 10.0

rtcsync

# Make steps in 100ms.
makestep 0.1 3
```

Then restart chrony:

```
systemctl start chrony
```

Next edit /etc/sysctl.conf to look like this:

```
fs.file-max = 10000000
fs.nr_open = 10000000

net.core.netdev_max_backlog = 100000
net.core.somaxconn = 100000
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.ip_local_port_range = 1024 65535
net.ipv4.ip_nonlocal_bind = 1
net.ipv4.tcp_fin_timeout = 10
net.ipv4.tcp_keepalive_time = 300
net.ipv4.tcp_max_orphans = 262144
net.ipv4.tcp_max_syn_backlog = 100000
net.ipv4.tcp_max_tw_buckets = 262144
net.ipv4.tcp_mem = 786432 1697152 1945728
net.ipv4.tcp_reordering = 3
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_sack = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_syn_retries = 5
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_wmem = 4096 16384 16777216

net.netfilter.nf_conntrack_max = 10485760
net.netfilter.nf_conntrack_tcp_timeout_fin_wait = 30
net.netfilter.nf_conntrack_tcp_timeout_time_wait = 15

vm.swappiness = 10
```

Afterwards, edit /etc/security/limits.conf to this:

```
root               soft    nofile            32768
<CHANGE TO THE USERNAME WHO RUNS JORMUNGANDR>            soft    nofile            32768
<CHANGE TO THE USERNAME WHO RUNS JORMUNGANDR>            hard    nofile            1048576
```








