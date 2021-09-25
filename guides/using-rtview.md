# Using RTView
RTView is a separate application: you should download, configure, and launch it separately from your node.
RTView is a program implemented in the Haskell programming language.
## Set up
First you will need to download a release package and unpack it, inside will be an executable cardano-rt-view

{% hint style="info" %}
macOS notes:
To prevent a system warning about "an application downloaded from the Internet", run this command:
```bash
xattr -r -d com.apple.quarantine cardano-rt-view-*-darwin/*
```
{% endhint %}

https://github.com/input-output-hk/cardano-rt-view/releases

Or you can build it from source, which will be explained more in depth here.

https://github.com/input-output-hk/cardano-rt-view/blob/master/doc/getting-started/building-rt-view-from-sources.md

## Configuration
This guide assumes that you already have at least one instance of cardano-node, if not go read the offical cardano documentation.

After you run an executable cardano-rt-view, an interactive dialog will be started:

```bash
RTView: real-time watching for Cardano nodes

Let's configure RTView...
```
The first question is:

```bash
How many nodes will you connect (1 - 99, default is 3):
```
Now input the amount of cardano-node process that will forward their metrics to RTView

Next question is:
```bash
Input the names of the nodes (default are "node-1", "node-2", "node-3"), one at a time:
```

From RTView's point of view, each cardano-node process that forwards its metrics to RTView, should be identified by a unique name. You can use any name you want. 
{% hint style="danger" %}
Note that the names must not include spaces or dots!
{% endhint %}

The next one is:
```bash
Indicate the port for the web server (1024 - 65535, default is 8024):
```

Please input the port RTView will use to display the web-page. For example, if you keep the default port 8024, the web-page will be available on http://127.0.0.1:8024.

The next question is:
```bash
Indicate how your nodes should be connected with RTView: networking sockets <S> or named pipes <P>."
Default way is sockets, so if you are not sure - choose <S>:
```
Please choose the way how the nodes should be connected to RTView.

If you chose S, you will be asked about the base port:
```bash
Ok, sockets will be used. Indicate the base port to listen for connections (1024 - 65535, default is 3000):
```
The base port will be used for the first node that forwards its metrics to RTView. For example, if you will launch three cardano-node processes (node-1, node-2, and node-3) that will forward their metrics using network sockets, this is how they will be connected to RTView:

node-1 -> 0.0.0.0:3000
node-2 -> 0.0.0.0:3001
node-3 -> 0.0.0.0:3002
Bit if you selected P, you will be asked about the directory when pipes will be created:
```bash
Ok, pipes will be used. Indicate the directory for them, default is "/run/user/1000/rt-view-pipes":
```
Next question is:
```bash
Now, indicate a host of machine RTView will be launched on (default is 0.0.0.0):
```
If your nodes are launched on the same machine with RTView, you can choose the default address. But if RTView will be launched on another machine, please specify its reachable public IP address. It will allow your nodes to connect with RTView. In this case, it is assumed that a machine with RTView is accessible using that IP address.

The last question is:
```bash
Indicate the directory with static content for the web server, default is "static":
```
Since RTView displays nodes' metrics on the web-page, it uses static web content (CSS, JS, images). By default, it's static directory that is included in the archive you've downloaded.
Then you will see this message:
```bash
Great, RTView is ready to run! Its configuration was saved at PATH_TO/rt-view.yaml. Press <Enter> to continue...
```
where PATH_TO is the full path to the default system configuration directory.
After you pressed Enter, RTView will show all the changes you have to make in your node(s) configuration file(s). For example, if you chose all default values on Linux, you will see this:
```bash
Now you have to make the following changes in your node's configuration file:

1. Find TurnOnLogMetrics flag and make sure it is true:

   "TurnOnLogMetrics": true

2. Since you have 3 nodes, add following traceForwardTo sections in the root of their configuration files:

   "traceForwardTo": {
     "tag": "RemoteSocket",
     "contents": [
       "0.0.0.0",
       "3000"
     ]
   }

   "traceForwardTo": {
     "tag": "RemoteSocket",
     "contents": [
       "0.0.0.0",
       "3001"
     ]
   }

   "traceForwardTo": {
     "tag": "RemoteSocket",
     "contents": [
       "0.0.0.0",
       "3002"
     ]
   }

After you are done, press <Enter> to run RTView...
```

After you pressed Enter, RTView will be launched, and you can open http://127.0.0.1:8024 (if you chose default web-port) and see the web-page.

help from: https://github.com/input-output-hk/cardano-rt-view
