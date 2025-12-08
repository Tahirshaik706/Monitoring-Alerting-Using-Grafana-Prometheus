# 🚀 AWS Monitoring & Alerting Stack
### Prometheus | Grafana | Alertmanager | Node Exporter | PagerDuty

This project sets up an end-to-end monitoring and alerting system for AWS EC2 servers.  
The stack collects metrics using Node Exporter, scrapes them using Prometheus, visualizes them in Grafana dashboards, and sends incident alerts to PagerDuty when thresholds are crossed.

---

## 📂 Components Used
| Tool | Purpose |
|------|---------|
| Prometheus | Metrics scraping & storage |
| Node Exporter | Server metrics exporter |
| Grafana | Visualization & dashboards |
| Alertmanager | Alert routing |
| PagerDuty | Incident notifications |

---

## 🏗 Architecture
Node Exporter  →  Prometheus  →  Alertmanager  →  PagerDuty Alerts ↓ Grafana UI


---

## 🚀 Installation (EC2)
```bash
git clone https://github.com/<your-username>/Monitoring-Alerting-Using-Grafana-Prometheus.git
cd Monitoring-Alerting-Using-Grafana-Prometheus
chmod +x setup.sh
sudo ./setup.sh


---

🔥 Prometheus Alerts Included

1️⃣ Instance Down Alert

ALERT InstanceDown
  IF up == 0
  FOR 1m
  LABELS { severity="critical" }
  ANNOTATIONS {
    summary="Instance down: {{ $labels.instance }}",
    description="The instance has been unreachable for more than 1 minute."
  }

2️⃣ High CPU Usage Alert

ALERT HighCPUUsage
  IF (100 - avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[2m]) * 100)) > 60
  FOR 2m
  LABELS { severity="critical" }
  ANNOTATIONS {
    summary="High CPU detected",
    description="CPU usage above 60% for more than 2 minutes."
  }

3️⃣ High Memory Usage Alert

ALERT HighMemoryUsage
  IF (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 75
  FOR 2m
  LABELS { severity="warning" }
  ANNOTATIONS {
    summary="High memory detected",
    description="Memory usage greater than 75%."
  }

4️⃣ High Disk Usage Alert

ALERT HighDiskUsage
  IF (node_filesystem_size_bytes - node_filesystem_free_bytes) / node_filesystem_size_bytes * 100 > 80
  FOR 3m
  LABELS { severity="critical" }
  ANNOTATIONS {
    summary="High disk usage detected",
    description="Disk usage greater than 80%."
  }

5️⃣ Unauthorized Requests Alert

ALERT UnauthorizedRequests
  IF increase(http_requests_total{status=~"401|403"}[5m]) > 0
  LABELS { severity="warning" }
  ANNOTATIONS {
    summary="Unauthorized requests detected",
    description="Requests with status 401 or 403 detected in last 5 minutes."
  }


---

📊 Grafana Dashboard Setup

Grafana → Dashboards → Import → Enter dashboard ID: 1860
Select datasource → prometheus


---

🔔 PagerDuty Integration

Alertmanager uses PagerDuty webhook to trigger incidents.

Alertmanager file location:

/etc/alertmanager/alertmanager.yml

Modify the routing:

receivers:
  - name: pagerduty
    pagerduty_configs:
      - routing_key: "<YOUR_PAGERDUTY_INTEGRATION_KEY>"


---

🛑 Service Commands

sudo systemctl restart prometheus
sudo systemctl restart alertmanager
sudo systemctl restart grafana-server
sudo systemctl restart node_exporter  # only on node server


---

🧪 Test Alerts

Alert	How to trigger

High CPU	yes > /dev/null &
Stop high CPU	pkill yes
Disk full	fallocate -l 5G test.img
Instance down	Stop node_exporter service



---

📌 Project Outcome

✔ Full AWS monitoring setup
✔ Auto-target discovery using EC2 SD
✔ Grafana dashboards showing live metrics
✔ PagerDuty incidents on failures
✔ Production-ready monitoring automation


---

🧑‍💻 Author

Shaik Tahir Basha
DevOps & Cloud Engineer

🌐 GitHub: https://github.com/Tahirshaik706
🔗 LinkedIn: https://www.linkedin.com/in/shaik-tahir-basha-6b5018270

---
