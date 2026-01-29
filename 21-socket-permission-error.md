# Permission Error

When you run `docker info` you might get the below error:

permission denied while trying to connect to the docker API at unix:///var/run/docker.sock

## Fix

This error is a classic "handshake" failure between your Java test and the Docker Engine you just installed.

The logs show that **Testcontainers** (the library Quarkus uses to start databases for tests) is trying to talk to Docker via the Unix socket (`/var/run/docker.sock`), but it's being told **"Permission Denied"** or **"Not Listening."**

Even though you have Docker running, your current user likely doesn't have the rights to access that socket file yet.

---

### The Diagnosis

Look at these specific lines in your log:

* `DOCKER_HOST unix:///var/run/docker.sock is not listening`
* `Could not find a valid Docker environment.`
* `Command "docker info" exited with error code 1.`

This confirms that while the `mvnw` process is running as `neutral00`, it doesn't have the "keys" to the Docker engine.

### The Solution: 3 Quick Steps

#### 1. Add your user to the Docker Group

You need to tell Linux that your user is allowed to talk to Docker without using `sudo`.

```bash
sudo usermod -aG docker $USER

```

#### 2. Apply the Group Change

For the group change to take effect, you usually have to log out and back in. However, you can "force" it in your current terminal session with:

```bash
newgrp docker

```

#### 3. Verify the Fix

Run this command **without sudo**. If it works, your Java tests will also work:

```bash
docker info

```

*If you see a bunch of system info, you're good. If you see "permission denied," the group change hasn't kicked in yet.*

---

### Why this happened

When you installed Docker Engine earlier, it created a "lock" on the communication pipe (`/var/run/docker.sock`) so only the `root` user or members of the `docker` group can use it. Since you were running `./mvnw verify` as a normal user, the test crashed the moment it tried to ask Docker to start a database.

### Pro-Tip: The "Quarkus Dev Services"

Quarkus is smart. In the background of your test, it's trying to use **Dev Services**. It sees your project needs a database, looks for Docker, and tries to "spin one up" automatically so you don't have to configure a connection string. Once you fix the permission issue, this error will disappear and your tests will magically have a database to talk to.

**Did `docker info` work without sudo after running those commands?**

[How to use Testcontainers with Quarkus](https://www.youtube.com/watch?v=BfR5YwEmg8I)
This video demonstrates how Quarkus integrates with Testcontainers and how to resolve common environmental issues when the container runtime is not detected.
