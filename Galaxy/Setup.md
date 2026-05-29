
## Prep for Galaxy Science Gateway Workshop
Allow 10–20 minutes to complete these steps and be sure to finish them *before the Monday afternoon session on June 1*.

> **If your current VM is a *tiny* or *small* flavor, stop it and launch a new instance** (minimum **`m3.quad`** or larger). The steps below assume you are already connected to a suitable VM.

---

### 1. Clone the Galaxy source  
Launch your Jetstream instance's web shell

```bash
cd ~
git clone -b release_26.1 https://github.com/galaxyproject/galaxy.git
cd galaxy
```

---

### 2. Configure Galaxy  

```bash
vi config/galaxy.yml
```

- Press `i` to start inserting text.  
- Add the following, inserting **your own e‑mail address** where indicated:

```yaml
gravity:
  gunicorn:
    bind: "0.0.0.0:8080"

galaxy:
  admin_users: your_email@address
```

Save and exit (`:wq` if using `vi`).

---

### 3. Redirect HTTP traffic to Galaxy’s port  

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDIRECT --to-port 8080
```
---

### 4. Start Galaxy  

```bash
sh run.sh
```

The first launch takes ~10 minutes. When the log shows

```
Serving on http://0.0.0.0:8080
```

Galaxy is ready.

---

### 5. Open Galaxy in a browser  

```
http://<vm‑public‑ip-address>
```
Look for your unique <vm‑public‑ip-address> on the Jetstream Exosphere interface under the Credentials card. 

---

### 6. Create the admin account  

Register a new Galaxy user **using the same e‑mail address** you placed in `galaxy.admin_users`. That account becomes a Galaxy **admin** automatically.

---  

You’re all set – the Galaxy instance is up and the admin account is ready for use.
