# Up and running the Systemd
{% hint style="info" %} 
The "TopologyUpdater" service, is intended to be a temporary solution to allow everyone to activate their relay nodes without having to postpone and wait for manual topology completion requests.
{% endhint %}

The topologyUpdater shell script must be executed on the relay node as a cronjob exactly every 60 minutes. After 4 consecutive requests (3 hours) the node is considered a new relay node in listed in the topology file. If the node is turned off, it's automatically delisted after 3 hours.

## Download and Configure

If you have run prereqs.sh, this should already be available in your scripts folder and would make this step unnecessary.
```bash
cd $CNODE_HOME/scripts
curl -s -o topologyUpdater.sh https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/topologyUpdater.sh
curl -s -o env https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/env
chmod 750 topologyUpdater.sh
./topologyUpdater.sh
```

## Examine and modify the variables within topologyUpdater.sh script

the script may come with some adjustments, that may or may not be valid for your environment. One of the common changes would be to the complete CUSTOM_PEERS section as below to include your local relays/BP nodes, and any additional peers you'd like to be always available at minimum. Take some time to update the variables in User Variables section in env & topologyUpdater.sh

## Systemd Service

The script can be deployed as a background service in different ways but the most recommended and easiest way is if prereqs.sh was used, utilize the deploy-as-systemd.sh script to setup and schedule the execution. This will deploy both push & fetch service files as well as timers for a scheduled 60 min node alive message and cnode restart at the user set time, with the default being 24 hours, when running the deploy script.
```bash
> cnode-tu-push.service : pushes a node alive message to Topology Updater API
> cnode-tu-push.timer : schedules the push service to execute once every hour
> cnode-tu-fetch.service : fetches a fresh topology file before the cnode.service file is started/restarted
> cnode-tu-restart.service : handles the restart of cardano-node (cnode.sh)
> cnode-tu-restart.timer : schedules the cardano-node restart service, default every 24h
```
this command can be used to to check the push and restart service schedule.
```bash
systemctl list-timers 
```
## Crontab job

Another way to deploy the topologyUpdater.sh script is as a crontab job. Add the script to be executed once per hour at a minute of your choice. The example below will handle both the fetch and push in a single call to the script once an hour. In addition to the below crontab job for topologyUpdater, it's expected that you also add a scheduled restart of the relay node to pick up a fresh topology file fetched by topologyUpdater script with relays that are alive and well.

```bash
25 * * * * /opt/cardano/cnode/scripts/topologyUpdater.sh
```
## Logs
You can check the last result of push message in 
```bash
logs/topologyUpdater_lastresult.json.
```
If deployed as systemd service, use 
```bash
sudo journalctl -u <service>
```
to check output from service.

If one of the parameters is outside the allowed ranges, invalid, or missing, the returned JSON will tell you what needs to be fixed.

help from: https://cardano-community.github.io/guild-operators/Scripts/topologyupdater/#deploy-the-script
