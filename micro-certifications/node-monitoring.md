# Node Monitoring

![image](https://user-images.githubusercontent.com/73615683/134784880-c3fe9577-06aa-48a1-bf4f-332f59679287.png)

## Grafana Information

Grafana is a several platform open source analytics and interactive visualization application. It can provide charts, graphs, and alerts when connected to supported data sources. Grafana does not require you to ingest data to a backend store or vendor database. Instead, grafana unifies your existing data, wherever it lives. With Grafana, you can also share the dashboards you create with other team members, allowing you to explore the data together. Grafana allows you to build dashboards specifically for you and your team, this is good for security and makes it easy to work and communicate with your team.

## RTView Information

RTView is an application that allows you to see the state of running Cardano nodes in real-time. Its useful for anyone who runs Cardano nodes and wants to see a good idea of what is going on. Some key features that make RTView desirable over others are: You can view the information about the nodes, peers, blockchain, transactions, resources, etc. you can connect multiple nodes, as many nodes as you want, whether they run locally or on different machines. You receive email notifications for any problems with your nodes. It even comes with a web-based UI, it provides configurable sections, live charts, and interactive tooltips. You can use any browser or run RTView behind a web server like NGINX and view your node states on any third-party device.

## GLiveview Information

Guild LiveView \(GLiveView\) is a local monitoring tool to be used in addition to remote monitoring tools like Prometheus, Grafana, or IOG's RTView. This is especially useful when moving to a systemd deployment if you haven't done so already, as it offers an intuitive UI to monitor the node status. GLiveView is independent from other files and can run as a standalone utility that can be stopped/started without affecting the status of cardano-node.

## Prometheus

Prometheus is important to monitoring your node because it queries your node and collects data Prometheus will scrape the metrics of your cardano nodes in a predefined time interval. Prometheus stores all the metrics it collects as time series data, i.e. metrics information is stored with the timestamp in the order it was recorded as, alongside optional key-value pairs called labels. Prometheus is designed for reliability, to be the system you go to during an outage to allow you to quickly diagnose problems. All of the prometheus server is able to stand alone, not depending on network storage or other remote services. You can rely on prometheus when your infrastructure isn’t working, and you do not need to set up extensive infrastructure to use it.

