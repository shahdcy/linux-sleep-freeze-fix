# linux-sleep-freeze-fix
 A fix for Linux systems freezing when waking from sleep due to i8042/keyboard issues.

````markdown
# Linux Mint Sleep Freeze Fix (NumLock Issue)

While using Linux Mint Cinnamon, I faced a freeze issue when NumLock was enabled.  
After troubleshooting, I identified that the default sleep state was set to **s2idle** instead of **deep**.  

This guide explains how I fixed it.

---

## Step 1: Verify Current Sleep State
```bash
cat /sys/power/mem_sleep
````

If the output shows:

```
[s2idle] deep
```

It means `deep` is not enabled.

---

## Step 2: Edit GRUB

Open the GRUB file:

```bash
sudo nano /etc/default/grub
```

Modify:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
```

To:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet mem_sleep_default=deep atkbd.reset i8042.reset i8042.nomux i8042.nopnp i8042.dumbkbd"
```

Save, then update GRUB and reboot:

```bash
sudo update-grub
sudo reboot
```

---

## Step 3: Adjust Power Settings

Go to:

* **Settings → Power Manager**
* Set suspend on lid close and power button (for both Battery and Plugged in modes).

---

## Step 4: Enable Lock Screen After Sleep

If using `xfce4-screensaver`:

```bash
xfconf-query -c xfce4-screensaver -p /lock/sleep-activation -s true
xfconf-query -c xfce4-session -p /general/LockCommand -s "xfce4-screensaver-command -l"
```

---

## Result

After applying these changes, my Linux Mint system no longer freezes when NumLock is enabled.
This solution can help others facing the same issue.
