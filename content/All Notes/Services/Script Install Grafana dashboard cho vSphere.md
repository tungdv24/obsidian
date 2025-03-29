# Script Install Grafana dashboard cho vSphere
29-03-2025
Tags: #services #monitor #docker

Install **Docker** về server và đảm bảo service đang running
[[Install Docker]] 
Tạo một folder bất kì và tạo những file sau

**docker-compose.yml**
```bash
version: "3"

services:
  influxdb:
    image: influxdb:2.1.1
    volumes:
      - influxdb-storage:/var/lib/influxdb2:rw
    env_file:
      - .env
    entrypoint: ["./entrypoint.sh"]
    restart: on-failure:10
    ports:
      - ${DOCKER_INFLUXDB_INIT_PORT}:8086

  telegraf:
    image: telegraf:1.19
    volumes:
      - ${TELEGRAF_CFG_PATH}:/etc/telegraf/telegraf.conf:rw
    env_file:
      - .env
    depends_on:
      - influxdb

  grafana:
    image: grafana/grafana
    volumes:
      - grafana-storage:/var/lib/grafana:rw
    depends_on:
      - influxdb
    ports:
      - ${GRAFANA_PORT}:3000

volumes:
  grafana-storage:
  influxdb-storage:
```

**.env**
```bash
DOCKER_INFLUXDB_INIT_MODE=setup

## Environment variables used during the setup and operation of the stack
#

# Primary InfluxDB admin/superuser credentials
#
DOCKER_INFLUXDB_INIT_USERNAME=     
DOCKER_INFLUXDB_INIT_PASSWORD=
DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=

# Primary InfluxDB organization & bucket definitions
# 
DOCKER_INFLUXDB_INIT_ORG=influxdb       
DOCKER_INFLUXDB_INIT_BUCKET=influxdb

# Primary InfluxDB bucket retention period
#
# NOTE: Valid units are nanoseconds (ns), microseconds(us), milliseconds (ms)
# seconds (s), minutes (m), hours (h), days (d), and weeks (w).
DOCKER_INFLUXDB_INIT_RETENTION=4d


# InfluxDB port & hostname definitions
#
DOCKER_INFLUXDB_INIT_PORT=8086
DOCKER_INFLUXDB_INIT_HOST=influxdb

# Telegraf configuration file
# 
# Will be mounted to container and used as telegraf configuration
TELEGRAF_CFG_PATH=./telegraf/telegraf.conf

# Grafana port definition
GRAFANA_PORT=3000
```

**entrypoint.sh**

```bash
#!/bin/bash

# Protects script from continuing with an error
set -eu -o pipefail

# Ensures environment variables are set
export DOCKER_INFLUXDB_INIT_MODE=$DOCKER_INFLUXDB_INIT_MODE
export DOCKER_INFLUXDB_INIT_USERNAME=$DOCKER_INFLUXDB_INIT_USERNAME
export DOCKER_INFLUXDB_INIT_PASSWORD=$DOCKER_INFLUXDB_INIT_PASSWORD
export DOCKER_INFLUXDB_INIT_ORG=$DOCKER_INFLUXDB_INIT_ORG
export DOCKER_INFLUXDB_INIT_BUCKET=$DOCKER_INFLUXDB_INIT_BUCKET
export DOCKER_INFLUXDB_INIT_RETENTION=$DOCKER_INFLUXDB_INIT_RETENTION
export DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=$DOCKER_INFLUXDB_INIT_ADMIN_TOKEN
export DOCKER_INFLUXDB_INIT_PORT=$DOCKER_INFLUXDB_INIT_PORT
export DOCKER_INFLUXDB_INIT_HOST=$DOCKER_INFLUXDB_INIT_HOST

# Conducts initial InfluxDB using the CLI
influx setup --skip-verify --bucket ${DOCKER_INFLUXDB_INIT_BUCKET} --retention ${DOCKER_INFLUXDB_INIT_RETENTION} --token ${DOCKER_INFLUXDB_INIT_ADMIN_TOKEN} --org ${DOCKER_INFLUXDB_INIT_ORG} --username ${DOCKER_INFLUXDB_INIT_USERNAME} --password ${DOCKER_INFLUXDB_INIT_PASSWORD} --host http://${DOCKER_INFLUXDB_INIT_HOST}:8086 --force
```

telegraf/telegraf.conf
```bash
# Read metrics from one or many vCenters
[[inputs.vsphere]]
  ## List of vCenter URLs to be monitored. These three lines must be uncommented
  ## and edited for the plugin to work.
  vcenters = [ "https://192.168.10.166/sdk" ]  # Use the correct vSphere API endpoint
  username = "monitor@lapbd.lab"
  password = "QGUeg([!K6!~"  # Escaped double quote is correct
  insecure_skip_verify = true
  timeout = "60s"


  ## VMs
  ## Typical VM metrics (if omitted or empty, all metrics are collected)
  # vm_include = [ "/*/vm/**"] # Inventory path to VMs to collect (by default all are collected)
  # vm_exclude = [] # Inventory paths to exclude
  vm_metric_include = [
    "cpu.demand.average",
    "cpu.idle.summation",
    "cpu.latency.average",
    "cpu.readiness.average",
    "cpu.ready.summation",
    "cpu.run.summation",
    "cpu.usagemhz.average",
    "cpu.used.summation",
    "cpu.wait.summation",
    "mem.active.average",
    "mem.granted.average",
    "mem.latency.average",
    "mem.swapin.average",
    "mem.swapinRate.average",
    "mem.swapout.average",
    "mem.swapoutRate.average",
    "mem.usage.average",
    "mem.vmmemctl.average",
    "net.bytesRx.average",
    "net.bytesTx.average",
    "net.droppedRx.summation",
    "net.droppedTx.summation",
    "net.usage.average",
    "power.power.average",
    "virtualDisk.numberReadAveraged.average",
    "virtualDisk.numberWriteAveraged.average",
    "virtualDisk.read.average",
    "virtualDisk.readOIO.latest",
    "virtualDisk.throughput.usage.average",
    "virtualDisk.totalReadLatency.average",
    "virtualDisk.totalWriteLatency.average",
    "virtualDisk.write.average",
    "virtualDisk.writeOIO.latest",
    "sys.uptime.latest",
  ]
  # vm_metric_exclude = [] ## Nothing is excluded by default
  # vm_instances = true ## true by default

  ## Hosts
  ## Typical host metrics (if omitted or empty, all metrics are collected)
  # host_include = [ "/*/host/**"] # Inventory path to hosts to collect (by default all are collected)
  # host_exclude [] # Inventory paths to exclude
  host_metric_include = [
    "cpu.coreUtilization.average",
    "cpu.costop.summation",
    "cpu.demand.average",
    "cpu.idle.summation",
    "cpu.latency.average",
    "cpu.readiness.average",
    "cpu.ready.summation",
    "cpu.swapwait.summation",
    "cpu.usage.average",
    "cpu.usagemhz.average",
    "cpu.used.summation",
    "cpu.utilization.average",
    "cpu.wait.summation",
    "disk.deviceReadLatency.average",
    "disk.deviceWriteLatency.average",
    "disk.kernelReadLatency.average",
    "disk.kernelWriteLatency.average",
    "disk.numberReadAveraged.average",
    "disk.numberWriteAveraged.average",
    "disk.read.average",
    "disk.totalReadLatency.average",
    "disk.totalWriteLatency.average",
    "disk.write.average",
    "mem.active.average",
    "mem.latency.average",
    "mem.state.latest",
    "mem.swapin.average",
    "mem.swapinRate.average",
    "mem.swapout.average",
    "mem.swapoutRate.average",
    "mem.totalCapacity.average",
    "mem.usage.average",
    "mem.vmmemctl.average",
    "net.bytesRx.average",
    "net.bytesTx.average",
    "net.droppedRx.summation",
    "net.droppedTx.summation",
    "net.errorsRx.summation",
    "net.errorsTx.summation",
    "net.usage.average",
    "power.power.average",
    "storageAdapter.numberReadAveraged.average",
    "storageAdapter.numberWriteAveraged.average",
    "storageAdapter.read.average",
    "storageAdapter.write.average",
    "sys.uptime.latest",
  ]
```

Verify các container đang chạy bằng lệnh docker ps

![[Ảnh màn hình 2025-03-29 lúc 14.14.21.png]]

Sau đó truy cập dashboard Grafana tại IP:3000

# Reference

https://github.com/huntabyte/tig-stack
https://github.com/jersonmartinez/docker-compose-influxdb-telegraf-grafana