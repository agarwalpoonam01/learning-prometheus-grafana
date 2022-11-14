==============================================


System Requirments:

Able to install virtualBox and vagrant
  
  
===============================================


Install virtualbox

Install Vagrant

git clone 

cd learning-prometheus-grafana/  

  < create required changes in Vagrant file like ip >
  
vagrant up

vagrant ssh


===============================================


From within the Vagrant machine:

  Able to install:
  
    Python3 and its dependencies python3 , python3-devel.x86_64 , pip3
    
  Access to and download files from:
  
    https://github.com/
    
    https://dl.grafana.com/
    

==============================================


Prometheus Installation

  cd /home/vagrant/synced_folder/training-prometheus/scripts/
  
  sudo sh install_prometheus.sh
  
  sudo systemctl start prometheus
  
  sudo systemctl enable prometheus
  
  sudo systemctl status prometheus
 
  Check Prometheus is accessible from the browser:  http://< vagrant-ip >:9090/


===============================================
 
 
Grafana installation

  cd /home/vagrant/
  
  wget https://dl.grafana.com/oss/release/grafana-9.2.4-1.x86_64.rpm
  
    sudo yum install grafana-9.2.4-1.x86_64.rpm
    
    sudo systemctl status grafana-server.service 
    
    sudo systemctl start grafana-server.service 
    
    sudo systemctl status grafana-server.service 
    
  Check Grafana is accessible from the browser:  http://< vagrant-ip >:3000/
  
  
=================================================


Exposing metrics with the help of Node Exporter:

cd /home/vagrant/synced_folder/training-prometheus/scripts/

sudo sh install_node_exporter.sh

sudo systemctl status node_exporter.service

sudo systemctl start node_exporter.service

sudo systemctl enable node_exporter.service

sudo vi /etc/prometheus/prometheus.yml 
```
  - job_name: 'node_exporter'
  
    scrape_interval: 5s
    
    static_configs:
    
      - targets: ['localhost:9100']

```
sudo systemctl restart prometheus


================================================

Custom exporter to expose sysstem metrics:


Python exporters that exposes :

  cd /home/vagrant/synced_folder/training-prometheus/scripts
  
  sudo yum install python3 -y
  
  sudo yum install python3-devel.x86_64 -y
  
  sudo pip3 install prometheus psutil
  
  metrics for cpu, memory
  
      nohup python3 memory-cpu.py &
      
  metrics for iops
  
      nohup python3 timing_write_io_example.py &
 
   
================================================


sudo vi /etc/prometheus/prometheus.yml 
```
  - job_name: "memory-cpu"
  
    static_configs:
    
      - targets: ["localhost:4444"]
      

  - job_name: "iops"
  
    static_configs:
    
      - targets: ["localhost:3333"]
      
```
sudo systemctl restart prometheus


===============================================


Create data source in grafana

Create dashboard add to panels one for each service


===============================================
