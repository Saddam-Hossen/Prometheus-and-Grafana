Here’s a complete, clear **documentation** to run your **Spring Boot JAR in Ubuntu (WSL/Linux)** as a **background service**, so it **exposes metrics for Prometheus** and can be visualized in **Grafana**.

---

## ✅ Overview

You’ll:

1. Run your Spring Boot JAR (e.g. `DeviceManagement-1.1.jar`) on a specific port (`3079`)
2. Expose `/actuator/prometheus` endpoint
3. Ensure it runs in **background** (after closing terminal)
4. Ensure Prometheus scrapes metrics from it
5. Connect it to Grafana

---

## 🧱 Prerequisites

* Ubuntu (or WSL)
* Java installed (`java -version`)
* Spring Boot JAR file (e.g., `DeviceManagement-1.1.jar`)
* Prometheus already running (e.g., on port 9091)
* Grafana installed and accessible at `http://localhost:3000`

---

## 📦 Step 1: Start Spring Boot JAR as Background Process

### ✅ Method 1: Using `nohup`


```bash
 // here we can access jar file  from windows (cd /mnt/d/Thymeleaf/DeviceManagement/DeviceManagement/target) or can keep to linux
  nohup java -jar DeviceManagement-1.1.jar --server.port=3079 > springboot.log 2>&1 &
```

* `nohup`: Prevents termination on logout
* `&`: Runs in background
* Output goes to `springboot.log`
* Check if it's running:

```bash
ps aux | grep DeviceManagement
```

### ✅ Optional: View logs

```bash
tail -f springboot.log
```

---

## 🔁 Step 2: Prometheus Configuration

In your `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'springboot-app'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['localhost:3079']
```

Then reload or restart Prometheus:

```bash
./prometheus --config.file=prometheus.yml --web.listen-address=":9091"
```

> Visit: [http://localhost:9091/targets](http://localhost:9091/targets) to verify Spring Boot app is UP.

---

## 📊 Step 3: Grafana Integration

### Step-by-step:

1. Visit: [http://localhost:3000](http://localhost:3000)
2. Login (default: `admin` / `admin`)
3. Go to **Settings → Data Sources → Add data source**
4. Select **Prometheus**
5. Set:

   * **URL**: `http://localhost:9091`
6. Click **Save & Test**

---

## 📈 Step 4: Import Grafana Dashboard

You can import a dashboard that visualizes Spring Boot metrics.

1. Go to **Create → Import**
2. Use a dashboard ID like `4701` (Spring Boot Actuator)
3. Select your Prometheus data source
4. Click **Import**

---

## 🛠️ Step 5: Auto Start on Boot (Optional)

If using WSL:

Create a shell script `start_springboot.sh`:

```bash
#!/bin/bash
nohup java -jar /path/to/DeviceManagement-1.1.jar --server.port=3079 > /path/to/springboot.log 2>&1 &
```

Then call it manually or through a cron job at startup.

---

## ✅ Summary

| Component       | Port | Status URL                                  |
| --------------- | ---- | ------------------------------------------- |
| Spring Boot App | 3079 | `http://localhost:3079/actuator/prometheus` |
| Prometheus      | 9091 | `http://localhost:9091/targets`             |
| Grafana         | 3000 | `http://localhost:3000`                     |

---

