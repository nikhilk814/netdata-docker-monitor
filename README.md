# netdata-docker-monitor
Netdata Docker setup

### Netdata Docker Monitoring
Monitor your system and application performance in real time using **Netdata** with Docker.
---
### Prerequisites

- **Docker** installed on your system.
- A CentOS VM or Linux host.
---

## Step 1: Check Docker Installation
Run the following command to verify Docker is installed:
```bash
docker --version
````
<img width="1920" height="1020" alt="1" src="https://github.com/user-attachments/assets/d40953b7-0427-473e-a6c4-c159228f73b7" />

You should see the Docker version displayed.

---

## Step 2: Run Netdata Container

Start Netdata in Docker with the following command:

```bash
docker run -d --name=netdata \
  -p 19999:19999 \
  --cap-add SYS_PTRACE \
  --security-opt apparmor=unconfined \
  netdata/netdata
```
<img width="1383" height="275" alt="2" src="https://github.com/user-attachments/assets/50f0d357-c6c1-4de9-bb2b-db7c8b044def" />

This will:
* Download the Netdata Docker image.
* Start the container in the background.
* Expose the dashboard on port `19999`.
---

## Step 3: Verify Container Status

Check that the Netdata container is running:

```bash
docker ps
```
You should see a container named `netdata` in the list.
<img width="1918" height="240" alt="3" src="https://github.com/user-attachments/assets/bd8d2596-b144-4348-9583-bcda0d822c20" />

---

## Step 4: Access the Dashboard

1. Open a web browser on your host machine.
2. Enter the VM’s IP address with port `19999`:

```
http://192.168:19999
```
<img width="1917" height="870" alt="4" src="https://github.com/user-attachments/assets/700f439b-b5a3-410c-a0ac-476fb4fa15f9" />
<img width="1912" height="811" alt="5" src="https://github.com/user-attachments/assets/2ffeaef8-c0f5-4e89-9416-b5ae7f57d9fb" />

You will see the Netdata real-time dashboard.

---

## Step 5: Access Netdata Logs

To check for errors or warnings:

1. Enter the Netdata container:

```bash
docker exec -it netdata bash
```

2. View logs inside the container:

```bash
cat /var/log/netdata/error.log
```
---

## Step 6: Enable Docker Monitoring

To monitor Docker containers’ metrics, Netdata needs access to the Docker socket:

1. Stop and remove the current Netdata container:

```bash
docker stop netdata
docker rm netdata
```
<img width="937" height="297" alt="docker mr" src="https://github.com/user-attachments/assets/77d4115e-e785-4a66-9e78-185e701c6426" />

2. Run Netdata again with Docker monitoring enabled:

```bash
docker run -d --name=netdata \
  -p 19999:19999 \
  --cap-add SYS_PTRACE \
  --security-opt apparmor=unconfined \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  netdata/netdata
```
<img width="968" height="240" alt="Screenshot 2025-10-02 125313" src="https://github.com/user-attachments/assets/beeed627-ea6e-4a02-85de-6ea348fc41a7" />

### Explanation of Options

* `--cap-add SYS_PTRACE` – Allows Netdata to trace system processes for detailed CPU and process metrics.
* `--security-opt apparmor=unconfined` – Disables AppArmor restrictions so Netdata can access all system stats.
* `-v /var/run/docker.sock:/var/run/docker.sock:ro` – Mounts the Docker socket (read-only) to allow monitoring of all running Docker containers.

Now your containers’ **CPU, memory, network, and disk I/O metrics** will appear on the dashboard.

<img width="1730" height="406" alt="Screenshot 2025-10-02 125342" src="https://github.com/user-attachments/assets/e2383ffd-f921-422e-981d-4a86a9548f17" />

---

## Step 7: Access Dashboard with Docker Stats

Open your browser:

```
http://192.168.27.132:19999
```

Scroll down to view live stats for **Docker Containers** alongside system metrics.

<img width="1847" height="705" alt="Screenshot 2025-10-02 130101" src="https://github.com/user-attachments/assets/024c6d24-1e13-473f-ba36-6baa56d8d1fb" />
<img width="1890" height="776" alt="Screenshot 2025-10-02 130300" src="https://github.com/user-attachments/assets/1a6fc92b-4b59-48ab-b614-12e7164ad84d" />

---

