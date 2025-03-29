28-03-2025
Tags: #services #monitor 

Install **Docker** về server và đảm bảo service đang running
[[Install Docker]]

```bash
# Create a volume
docker volume create uptime-kuma

# Start the container
docker run -d --restart=always -p 3001:3001 -v uptime-kuma:/app/data --name uptime-kuma louislam/uptime-kuma:1

```

### Một số script sử dụng để add monitors với Kuma

**Sử dụng môi trường ảo venv**
[[Install venv]]

**Script sử dụng để add một dải IP vào KumaUptime**
```python
import ipaddress
from uptime_kuma_api import UptimeKumaApi, MonitorType #Script chạy IP theo dải

# Replace with your Kuma Uptime server details
KUMA_API_URL = 'http://192.168.10.144:3001'
KUMA_USERNAME = 'admin'
KUMA_PASSWORD = 'zREuiVfDFvLkCqA'

def add_monitors_in_range(cidr):
    net = ipaddress.ip_network(cidr)

    # Connect to Uptime Kuma and login
    with UptimeKumaApi(KUMA_API_URL) as api:
        try:
            api.login(KUMA_USERNAME, KUMA_PASSWORD)
            print("Successfully logged in to Uptime Kuma.")
        except Exception as e:
            print(f"Failed to log in: {e}")
            return

        # Loop through each IP in the specified range
        for ip in net.hosts():
            try:
                # For PING, use the IP as both the hostname and URL
                api.add_monitor(
                    type=MonitorType.PING,  # PING monitor type
                    name=f"Ping {ip}",
                    url=f"{ip}",  # Use IP as the "URL"
                    hostname=str(ip),  # Use IP as the "hostname"
                    interval=60,  # Check every minute
                    timeout=10
                )
                print(f"Successfully added monitor for {ip}.")
            except Exception as e:
                print(f"Error adding monitor for {ip}: {e}")

if __name__ == "__main__":
    # Specify your CIDR range here
    CIDR_RANGE = '192.168.10.0/24'
    add_monitors_in_range(CIDR_RANGE)
```
Tuy nhiên, việc add quá nhiều IP vào Kuma Uptime có thể khiến cho web bị giật lag và tốn nhiều thời gian hơn để load. Nên nếu phù hợp chỉ cần add một số IP quan trọng như Gateway hoặc IP server vật lí đơn lẻ

**Script add từng IP vào KumaUptime**
```python
from uptime_kuma_api import UptimeKumaApi, MonitorType #Script chạy theo từng IP

# Replace with your Kuma Uptime server details
KUMA_API_URL = 'http://192.168.10.144:3001'
KUMA_USERNAME = 'admin'
KUMA_PASSWORD = 'zREuiVfDFvLkCqA'

def add_monitors_from_list(ip_list):
    # Connect to Uptime Kuma and login
    with UptimeKumaApi(KUMA_API_URL) as api:
        try:
            api.login(KUMA_USERNAME, KUMA_PASSWORD)
            print("Successfully logged in to Uptime Kuma.")
        except Exception as e:
            print(f"Failed to log in: {e}")
            return

        # Loop through each IP in the specified list
        for ip in ip_list:
            try:
                # For PING, use the IP as both the hostname and URL
                api.add_monitor(
                    type=MonitorType.PING,  # PING monitor type
                    name=f"Ping Check {ip}",
                    url=f"{ip}",  # Use IP as the "URL"
                    hostname=str(ip),  # Use IP as the "hostname"
                    interval=60,  # Check every minute
                    timeout=10
                )
                print(f"Successfully added monitor for {ip}.")
            except Exception as e:
                print(f"Error adding monitor for {ip}: {e}")

if __name__ == "__main__":
    # Manually specify your IP addresses here
    ip_list = [
        '192.168.10.1',
        '192.168.10.2',
        '192.168.10.3',
        # Add more IPs as needed
    ]

    add_monitors_from_list(ip_list)

```
# References
https://hub.docker.com/r/louislam/uptime-kuma
https://github.com/louislam/uptime-kuma
