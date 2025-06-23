
---

# 📘 Documentation: Installing and Using Grafana with Prometheus

---

## 🖥️ Prerequisites

* ✅ Ubuntu / WSL (Windows Subsystem for Linux)
* ✅ Prometheus installed and running (e.g., on port `9091`)
* ✅ Spring Boot apps exposing metrics at `/actuator/prometheus`

---

## ✅ Step 1: Install Grafana on Ubuntu / WSL

### 🔹 1. Add Grafana APT repository

```bash
sudo apt update
sudo apt install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

### 🔹 2. Add Grafana GPG key

```bash
sudo apt install -y gnupg2 curl
curl https://packages.grafana.com/gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/grafana.gpg
```

### 🔹 3. Update and install Grafana

```bash
sudo apt update
sudo apt install grafana
```

---

## ✅ Step 2: Start and Enable Grafana Server

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

* ✅ Grafana runs by default on port `3000`
* Open: [http://localhost:3000](http://localhost:3000)

---

## ✅ Step 3: Log in to Grafana

* URL: `http://localhost:3000`
* **Username**: `admin`
* **Password**: `admin` (you’ll be asked to change it)

---

## ✅ Step 4: Add Prometheus as a Data Source

1. Click **⚙️ (gear icon)** → **Data Sources**
2. Click **“Add data source”**
3. Choose **Prometheus**
4. Set URL to:

   ```
   http://localhost:9091
   ```
5. Click **Save & Test**

---

## ✅ Step 5: Create a Dashboard

1. Click **📊 Dashboards** → **+ Create** → **Dashboard**
2. Click **“Add new panel”**
3. Enter a PromQL query, such as:

   ```
   http_server_requests_seconds_count
   ```
4. Click **Apply**
5. Save the dashboard

---

## ✅ Sample PromQL Queries (for Spring Boot)

| 📈 Purpose          | 🧪 Query                                                                                    |
| ------------------- | ------------------------------------------------------------------------------------------- |
| Total HTTP Requests | `http_server_requests_seconds_count`                                                        |
| Response Time (avg) | `rate(http_server_requests_seconds_sum[1m]) / rate(http_server_requests_seconds_count[1m])` |
| Memory Usage        | `jvm_memory_used_bytes`                                                                     |
| CPU Usage           | `process_cpu_usage`                                                                         |
| Uptime              | `process_uptime_seconds`                                                                    |

> These require Spring Boot with **Micrometer + Actuator**.

---

## ✅ Step 6: Optional - Import a Prebuilt Dashboard

1. Go to [Grafana Dashboards](https://grafana.com/grafana/dashboards/)
2. Search for:
   ➤ **“Spring Boot”**
   ➤ Dashboard ID: `4701` or `10280`
3. In Grafana, click:

   ```
   Dashboards → + Import
   ```
4. Enter Dashboard ID → Click **Load**
5. Choose your Prometheus data source

---

## 🛠️ Managing Grafana

| Command                                 | Description        |
| --------------------------------------- | ------------------ |
| `sudo systemctl start grafana-server`   | Start Grafana      |
| `sudo systemctl stop grafana-server`    | Stop Grafana       |
| `sudo systemctl restart grafana-server` | Restart Grafana    |
| `sudo systemctl enable grafana-server`  | Auto-start on boot |

---

## ✅ Summary

| Component           | URL                                            |
| ------------------- | ---------------------------------------------- |
| Grafana UI          | [http://localhost:3000](http://localhost:3000) |
| Prometheus          | [http://localhost:9091](http://localhost:9091) |
| Spring Boot metrics | `/actuator/prometheus`                         |

---

