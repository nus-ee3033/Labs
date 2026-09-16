# SSH Key Setup Guide

!!! info 
    SSH keys allow you to connect to the Raspberry Pi without entering the account password every time.

    Each Ubuntu VM should generate its **own SSH key**. Do not share your private SSH key with anyone.

---

# 1. Student Guide: Set Up an SSH Key

Complete the following steps **inside your Ubuntu virtual machine**.

## Step 1 — Generate an SSH key

Open a terminal and run:

```bash
ssh-keygen -t ed25519 -C "AY<YYYY>S<S>"
```

Replace `<YYYY>` with the current acedemic year and `<S>` with the current semester.

For example, for Acedemic Year 2026/2027, Semester 1:

```bash
ssh-keygen -t ed25519 -C "AY2627S1"
```

The comment allows you to identify and clear your student SSH keys at the end of the semester.

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

* `id_ed25519` is your **private key**. Do not share it.
* `id_ed25519.pub` is your **public key**. This is the key that will be added to the Raspberry Pi.

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

    * create a new Ubuntu VM,
    * reinstall the VM, or
    * connect from another computer,

    generate a **new SSH key** on that machine and repeat the setup above.

    Do not copy your private key between machines unless specifically instructed to do so.

---

# 2. Clearing Student SSH Keys

All SSH public keys added to the `balance` account are stored in:

```text
/home/balance/.ssh/authorized_keys
```

Each public key normally occupies one line. Student keys generated using the instructions above will end with a comment such as:

```text
AY2627S1
```

## Check Existing Student Keys

Before removing anything, you can list the student keys for the semester using:

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

To remove all student keys labelled for `AY2627S1`, run:

```bash
sed -i 'AY<YYYY>S<S>/d' /home/balance/.ssh/authorized_keys
```

For example: 
```bash
sed -i 'AY2627S1/d' /home/balance/.ssh/authorized_keys
```

This removes only lines tagged with:

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

## File Permissions

If required, restore the correct ownership and permissions with:

```bash
sudo chown balance:balance /home/balance/.ssh/authorized_keys
sudo chmod 700 /home/balance/.ssh
sudo chmod 600 /home/balance/.ssh/authorized_keys
```
