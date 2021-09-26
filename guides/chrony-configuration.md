# Chrony Configuration

To start go to [https://chrony.tuxfamily.org/download.html](https://chrony.tuxfamily.org/download.html) and download the latest version of chrony.

Then edit the /etc/chrony/chrony.conf file to look like this:

```text
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

```text
systemctl start chrony
```

Next edit /etc/sysctl.conf to look like this:

```text
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

```text
root               soft    nofile            32768
<CHANGE TO THE USERNAME WHO RUNS JORMUNGANDR>            soft    nofile            32768
<CHANGE TO THE USERNAME WHO RUNS JORMUNGANDR>            hard    nofile            1048576
```

If your service starts with systemctl then edit /etc/systemd/system/.service to something along the lines of:

```text
[Unit]
Description=Shelley Staking Pool
After=multi-user.target

[Service]
Type=simple
ExecStart=<YOUR_NODE_START_SCRIPT>

# 16K Or more
LimitNOFILE=16384

Restart=on-failure
RestartSec=5s
User=<CHANGE TO THE USER ID WHO RUNS JORMUNGANDR>
Group=users

[Install]
WantedBy=multi-user.target
```

And run:

```text
systemctl daemon-reload && systemctl enable shelley && systemctl start shelley
```

{% hint style="info" %}
Remove comment above LimitNOFILE
{% endhint %}

Start script example:

```text
#!/bin/bash

CONF_DIR=/<path>/<to>/<the>/<jormungandr>/<configfiles>
JORMUNGANDR=/<path>/<to>/<the>/<jormungandr>/<binaries>/jormungandr
# ITN V1 Block hash
BLOCK_HASH="8e4d2a343f3dcf9330ad9035b3e8d168e6728904262f2c434a4f8f934ec7b676"
LOGLEVEL="--log-level warn"
LOGOUTPUT="--log-output syslog"
export BLOCK_HASH  PATH=${CONF_DIR}:${PATH}


${JORMUNGANDR} --config ${CONF_DIR}/config.yaml --secret ${CONF_DIR}/secret.yaml  --genesis-block-hash $BLOCK_HASH ${LOGLEVEL} ${LOGOUTPUT}
```

Jormungandr's config.yaml for reference:

```text
max_connections: 1024 # Try with 1024 first and then 2048, 4192 or even 16384 (for the beast nodes)
  max_unreachable_nodes_to_connect_per_event: 128
  topics_of_interest:
    blocks: high
    messages: high
```

{% hint style="info" %}
Set the log level in config.yaml to "warn" or even "off"
{% endhint %}

Sort the following trusted peers by your current location and comment out the others:

```text
  # IOHK US West - San francisco, California
    - address: "/ip4/52.9.132.248/tcp/3000"
      id: 671a9e7a5c739532668511bea823f0f5c5557c99b813456c
    # IOHK US West - San francisco, California
    - address: "/ip4/52.8.15.52/tcp/3000"
      id: 18bf81a75e5b15a49b843a66f61602e14d4261fb5595b5f5
    ## IOHK EU Central - Frankfurt, Germany
    - address: "/ip4/52.28.91.178/tcp/3000"
      id: 23b3ca09c644fe8098f64c24d75d9f79c8e058642e63a28c
    # IOHK EU Central - Frankfurt, Germany
    - address: "/ip4/3.125.75.156/tcp/3000"
      id: 22fb117f9f72f38b21bca5c0f069766c0d4327925d967791
    ## IOHK AP North East - Tokyo, Japan
    - address: "/ip4/13.114.196.228/tcp/3000"
      id: 7e1020c2e2107a849a8353876d047085f475c9bc646e42e9
    # IOHK AP North East - Tokyo, Japan
    - address: "/ip4/13.112.181.42/tcp/3000"
      id: 52762c49a84699d43c96fdfe6de18079fb2512077d6aa5bc
    # IOHK EU Central - Frankfurt, Germany
    - address: "/ip4/3.124.116.145/tcp/3000"
      id: 99cb10f53185fbef110472d45a36082905ee12df8a049b74
```

Finally, allow the bare minimum of firewall allowance with this:

```text
ufw allow <YOUR_NODE_LISTENING_PORT: default: 3000>/tcp
ufw allow <WHERE YOUR SSH SRV IS LISTENING default:22>/tcp
```

This guide was heavily inspired by [https://gist.github.com/ilap/54027fe9af0513c2701dc556221198b2](https://gist.github.com/ilap/54027fe9af0513c2701dc556221198b2)

