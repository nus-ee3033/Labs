# (Almost Painless) SSH Setup Guide

# 1. Student Guide: Set Up an SSH Key

!!! info

    SSH keys allow you to connect to the Raspberry Pi without entering the account password every time.

    Each Ubuntu VM should generate its **own SSH key**. Do not share your private SSH key with anyone.

---

Complete the following steps **inside your Ubuntu virtual machine**.

## Step 1 — Generate an SSH Key

Open a terminal and run:

```bash
ssh-keygen -t ed25519 -C "AY<YYYY>S<S>"
```

Replace `<YYYY>` with the current academic year and `<S>` with the current semester.

For example, for Academic Year 2026/2027, Semester 1:

```bash
ssh-keygen -t ed25519 -C "AY2627S1"
```

The comment allows the teaching team to identify and clear student SSH keys from a particular semester.

You will see a prompt similar to:

```text
Enter file in which to save the key (/home/username/.ssh/id_ed25519):
```

Press **Enter** to use the default location.

You will then be asked for a passphrase. For the lab setup, you may press **Enter** to leave it empty.

This creates two files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

- `id_ed25519` is your **private key**. Do not share it.
- `id_ed25519.pub` is your **public key**. This is the key that will be added to the Raspberry Pi.

---

## Step 2 — Copy Your Public Key to the Raspberry Pi

Run:

```bash
ssh-copy-id balance@balanceX.local
```

where `X` is the number assigned to your Raspberry Pi.

Alternatively, you may connect using its IP address:

```bash
ssh-copy-id balance@192.168.1.xx
```

Enter the password `030609` when prompted.

You should only need to enter this password once.

---

## Step 3 — Test the Connection

Run:

```bash
ssh balance@balanceX.local
```

or:

```bash
ssh balance@192.168.1.xx
```

You should now be able to log in without entering the Raspberry Pi password.

---

!!! tip

    ### Using a Different VM or Computer

    SSH keys belong to the machine or VM on which they were generated.

    If you:

    - create a new Ubuntu VM,
    - reinstall the VM, or
    - connect from another computer,

    generate a **new SSH key** on that machine and repeat the setup above.

    Do not copy your private key between machines unless specifically instructed to do so.

---

# 2. SSH Shortcuts

!!! info

    You can create SSH shortcuts in your Ubuntu VM so that you do not need to type the full hostname or start the ROS 2 Docker container every time.

On your VM, open:

```bash
nano ~/.ssh/config
```

Add:

```ssh
Host <nickname>
    HostName balanceX.local
    User balance

Host <nickname>-ros
    HostName balanceX.local
    User balance
    RequestTTY force
    RemoteCommand bash -lc 'docker start ros2_humble >/dev/null 2>&1 || true; docker exec -it -w /home/ros2_ws ros2_humble bash'
```

Replace `<nickname>` with your preferred shortcut and `X` with the number assigned to your Raspberry Pi. For this manual, we will use `car` as the shortcut.

Set the correct permission for the SSH configuration file:

```bash
chmod 600 ~/.ssh/config
```

You can now use:

```bash
ssh car
```

to open a normal SSH session on the Raspberry Pi.

Or use:

```bash
ssh car-ros
```

to enter the ROS 2 Docker container directly at:

```text
/home/ros2_ws
```

The `car-ros` shortcut will start the `ros2_humble` container automatically if it is not already running.

---

# 3. Clearing Student SSH Keys

All SSH public keys added to the `balance` account are stored in:

```text
/home/balance/.ssh/authorized_keys
```

Each public key normally occupies one line. Student keys generated using the instructions above will end with a comment such as:

```text
AY2627S1
```

## Check Existing Student Keys

Before removing anything, you can list the student keys for a particular semester using:

```bash
grep "AY<YYYY>S<S>" /home/balance/.ssh/authorized_keys
```

For example:

```bash
grep "AY2627S1" /home/balance/.ssh/authorized_keys
```

This only displays matching keys and does not modify the file.

---

## Remove Student Keys for the Semester

To remove all student keys for a particular semester, run:

```bash
sed -i '/AY<YYYY>S<S>/d' /home/balance/.ssh/authorized_keys
```

For example, to remove all keys labelled `AY2627S1`:

```bash
sed -i '/AY2627S1/d' /home/balance/.ssh/authorized_keys
```

This removes only lines containing:

```text
AY2627S1
```

Other SSH keys in the file are left unchanged.

You can verify that the student keys have been removed using:

```bash
grep "AY<YYYY>S<S>" /home/balance/.ssh/authorized_keys
```

If there is no output, no matching student keys remain.

You can also inspect all remaining authorised keys using:

```bash
cat /home/balance/.ssh/authorized_keys
```

---

!!! tip

    ## File Permissions

    If required, restore the correct ownership and permissions with:

    ```bash
    sudo chown balance:balance /home/balance/.ssh/authorized_keys
    sudo chmod 700 /home/balance/.ssh
    sudo chmod 600 /home/balance/.ssh/authorized_keys
    ```