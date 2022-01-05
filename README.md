# Prometheus-NodeExporter-Grafana deployment using Ansible

Prometheus is an open-source monitoring system which is very lightweight and has a good alerting mechanism. Prometheus can monitor hundreds of instances and display metrics and statistics from all of them to you in one simple dashboard. 

Grafana is a web application that gives you visualization tools. In a nutshell, we will be able to create and use dashboards from Grafana to visualize all the statistics and metrics presented by Prometheus.

## Project Scope

In this demo, we will be creating an Ansible playbook to automate the installation of grafana, node_exporter, and Prometheus into an EC2 instance.

## Architecture

This is a monitoring project which consists of the following components:

- Node Exporter : Node exporter produces metrics about infrastructure which include CPU, memory, disc, network stats etc.
- Prometheus : It helps monitor servers which are called targets. It can be a single target or multiple targets monitored for different metrics.
- Grafana : Is a tool for visualising the metrics by connecting with the prometheus server.

In this module, we will see how each of these components can be automatically deployed using ansible playbook.

![image](https://user-images.githubusercontent.com/93197553/148266068-81e63e4f-744e-4bf1-9dde-734486b7ecb4.png)

We will be deploying this project using 3 instances.

- In the first instance, we will be setting this up as a monitoring server in which we will be installing prometheus(port:9090) and Grafana(port:3000) application. This instance connects to other 2 instances and reads the metrices like cpu usage,memory, stats etc in a timely manner.

- In the other 2 nodes, we will be setting up the node exporter which will be working on port 9100.

## Requirements

- Install Ansible on your machine (ansible master).
- 3 instances for installing node exporter, prometheus and grafana applications.

## Provisioning

#### Install and configure - Node Exporters

1. Download and extract the latest node exporter package

![image](https://user-images.githubusercontent.com/93197553/148269026-89f43035-b0ee-4a82-901b-2cd46534c5a4.png)

2. Creating a custom node exporter service(node_exporter.service) and start/enable the node exporter service.

![image](https://user-images.githubusercontent.com/93197553/148269155-2fab2ded-5324-4357-a2b4-682b29a622cf.png)

3. Now, node exporter would be exporting metrics on port 9100. You may view the server metrics as shown below.
```
http://<server-IP>:9100/metrics
```

#### Install and configure - Prometheus

1. Download and extract the latest binary for prometheus. Also, give required permisions and ownership for the files.

![image](https://user-images.githubusercontent.com/93197553/148270081-5cbd54c9-d935-4f74-9a9e-b438aea4e984.png)

2. Setting up binaries for prometheus.

![image](https://user-images.githubusercontent.com/93197553/148270614-023813b9-0400-42ad-be2a-1b9c5f32dd56.png)

3. Setup Prometheus Configuration, Prometheus Service File and start the service.

![image](https://user-images.githubusercontent.com/93197553/148271094-b6770ee4-4016-4d5d-a553-0c4011f14f2d.png)

4. Now you will be able to access the prometheus UI on 9090 port of the prometheus server.
```
http://<monitor-ip>:9090/targets
```

#### Install and configure - Grafana

1. Add a new YUM respository for the operating system to know where to download Grafana. Here, we will be using a custom repo for installing grafana.

![image](https://user-images.githubusercontent.com/93197553/148271562-e8c7799b-33f9-4558-bc62-1fea4baae542.png)

2. Install the grafana repo and start the grafana service.

![image](https://user-images.githubusercontent.com/93197553/148271828-49fe4176-76a8-4c4b-a4f4-1c94f8009c31.png)

3. Access grafana UI on port 3000.
```
http://<monitor_IP>:3000
```
4. Once grafana is installed, you may access the dashboard using the default credentials (username:admin and password:admin). After accessing grafana, you will a page for setting the new password. Make sure to use this password in the variable section. 

5. You may either manually add the prometheus datasource or this can also be automatically added using the 'grafana_datasource' module in ansible.

![image](https://user-images.githubusercontent.com/93197553/148272891-a77e9adf-3b7c-4f0c-8912-56687a27905c.png)

## Result

### Node Exporter - 
![image](https://user-images.githubusercontent.com/93197553/148274787-17ee7af6-b7e6-44c6-ae78-a32b66a9661e.png)

![image](https://user-images.githubusercontent.com/93197553/148275549-0c315fe0-11dc-401e-bd3d-ba7b9cd903f2.png)

### prometheus

![image](https://user-images.githubusercontent.com/93197553/148275719-36843a81-b8cb-4cc0-a8b6-e6ea41050d79.png)

![image](https://user-images.githubusercontent.com/93197553/148276026-1800c373-247a-4b76-8bb6-ff521d01c7a2.png)


### Grafana



