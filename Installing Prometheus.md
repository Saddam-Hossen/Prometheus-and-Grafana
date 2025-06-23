
---

# 📘 Documentation: Installing Prometheus on Port `9091` (Ubuntu / WSL)

---

## ✅ 1. **Download & Extract Prometheus**

```bash
cd ~
wget https://github.com/prometheus/prometheus/releases/download/v2.52.0/prometheus-2.52.0.linux-amd64.tar.gz
tar -xvzf prometheus-2.52.0.linux-amd64.tar.gz
cd prometheus-2.52.0.linux-amd64
```

---

## ✅ 2. **Edit the Prometheus Configuration File**

Open `prometheus.yml` in your editor:

```bash
nano prometheus.yml
```

Replace the contents with this example:

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9091"]

  - job_name: "springboot-app1"
    metrics_path: "/actuator/prometheus"
    static_configs:
      - targets: ["localhost:3079"]

  - job_name: "springboot-app2"
    metrics_path: "/actuator/prometheus"
    static_configs:
      - targets: ["localhost:3080"]
```

> Customize `targets` based on your app ports.

---

## ✅ 3. **Run Prometheus on Port `9091`**

From inside the Prometheus directory:

```bash
./prometheus --config.file=prometheus.yml --web.listen-address=":9091"
```

You should see:

```
Start listening for connections address=:9091
```

Then visit:
👉 `http://localhost:9091`

---

## ❗ Common Error & Fix

### 🔴 Error: `opening storage failed: lock DB directory`

**Cause**: Prometheus was stopped improperly or another process is still using the data folder.

### ✅ Fix Steps:

1. **Check running Prometheus process**:

   ```bash
   ps aux | grep prometheus
   ```

2. **Kill it if running**:

   ```bash
   kill -9 <PID>
   ```

3. **Delete lock file**:

   ```bash
   rm -f data/lock
   ```

4. **Start Prometheus again**:

   ```bash
   ./prometheus --config.file=prometheus.yml --web.listen-address=":9091"
   ```

---

## ✅ 4. **Verify Targets**

Visit:

```
http://localhost:9091/targets
```

Check that all `springboot-app` targets show `UP`.

---

## ✅ Optional: Run Prometheus in Background

```bash
nohup ./prometheus --config.file=prometheus.yml --web.listen-address=":9091" > prometheus.log 2>&1 &
```

Check log:

```bash
tail -f prometheus.log
```

---

## 📁 Folder Overview

After setup, your Prometheus folder looks like:

```
~/prometheus-2.52.0.linux-amd64/
├── data/
├── prometheus.yml
├── prometheus
├── promtool
...
```

---

## 📎 Tips

* Prometheus default UI: [http://localhost:9091](http://localhost:9091)
* Target health: [http://localhost:9091/targets](http://localhost:9091/targets)
* Query metrics: [http://localhost:9091/graph](http://localhost:9091/graph)

---

