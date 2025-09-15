# Install Prometheus with Docker volume
1. Create Docker volume 
```bash
docker volume create prometheus-data
```
Run docker prometheus with volume
```bash
docker run \
    -p 9090:9090 \
    -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
    -v prometheus-data:/prometheus \
    prom/prometheus
```
or
```bash
docker run -p 9090:9090 prom/prometheus
```
access on local browsr with port 9090

# Install Grafana with Docker volume
1. Create Docker volume 
```bash
docker volume create grafana-storage
```
Verify the volume has created
```bash
docker volume instpect grafana-storage
```
If you see the ouput json. it's already installed

2. Run Grafana with the Volume
```bash
docker run -d -p 3000:3000 --name=grafana \
  --volume grafana-storage:/var/lib/grafana \
  grafana/grafana-enterprise
```
access on local browser port 3000
