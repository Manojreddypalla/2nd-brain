# fan speed

### 🧰 Step 1: Install fan control tools

First, open a terminal and install the needed packages:

```bash
sudo apt update
sudo apt install macfanctld lm-sensors

```

- **`macfanctld`** – automatic fan controller designed for Macs running Linux.
- **`lm-sensors`** – helps monitor temperatures and fan sensors.

---

### ⚙️ Step 2: Check sensors

Run:

```bash
sudo sensors-detect

```

Keep pressing **Enter** to accept defaults, then run:

```bash
sensors

```

You should see your CPU/GPU temps — make sure it detects the fans.

### aggressive cooling

You can tweak macfanctld’s settings:

```bash
sudo nano /etc/macfanctl.conf

```

Change values like:

```
min_fan1_speed: 2000
max_fan1_speed: 4800
fan1_temp: 45
fan1_max_temp: 70

```

→ This makes fans ramp up sooner when your CPU hits ~45 °C.

Then restart:

```bash
sudo systemctl restart macfanctld

```