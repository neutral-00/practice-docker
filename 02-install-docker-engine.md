# Install Docker Engine


Here is the cleanest way to install **Docker Engine** on Ubuntu without the overhead of the Desktop GUI.

---

### Step 1: Clean up old versions

Even if you haven't installed it yet, Ubuntu sometimes comes with "snap" versions or older forks. Let's clear the deck:

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)

```

### Step 2: Set up the Docker Repository

We want the official Docker repo to get the most frequent updates.

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

### Step 3: Install Docker Engine

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```

### Step 4: The "Non-Root" Fix (Highly Recommended)

By default, you have to type `sudo` for every docker command. To run docker as your normal user:

1. **Create the group:** `sudo groupadd docker` (It might already exist).
2. **Add yourself:** `sudo usermod -aG docker $USER`
3. **Apply changes:** Run `newgrp docker` or log out and back in.

---

### Verification

Run the classic test to ensure the engine is pulling images and handling the networking (iptables) correctly:

```bash
docker run hello-world

```

### One Final Note on Your Firewall

Since you were concerned about `iptables`, remember this: If you use `ufw` to "deny all," your Docker containers will **still be reachable** if you use the `-p` flag.

> **Rule of thumb:** If you want a container to be private to your laptop only, bind it to localhost:
> `docker run -p 127.0.0.1:8080:8080 my-container`

