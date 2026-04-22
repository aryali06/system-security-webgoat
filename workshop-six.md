# Workshop 6
## Metasploit Tools and Functions
**Tools used:** Metasploit Framework, Metasploitable 2, VMware Workstation Pro

---

#### What I Did

This workshop introduced penetration testing using the **Metasploit Framework** — a tool used to identify and exploit known vulnerabilities in a target system. Rather than testing against WebGoat, this workshop used a dedicated vulnerable target: **Metasploitable 2**, an intentionally insecure Ubuntu Linux VM.

**Environment Setup**

I ran two VMs simultaneously inside VMware:
- **Kali Linux** — the attacker machine running Metasploit
- **Metasploitable 2** — the target machine

Both VMs were configured to use a **Host-Only network adapter**, keeping all traffic isolated from the real network. This is an important safety step — Metasploitable 2 is intentionally full of vulnerabilities and should never be exposed to an external network.

The default Metasploitable 2 credentials are `msfadmin / msfadmin`.

**Launching Metasploit**

From the Kali terminal, I launched the Metasploit console:

```bash
./msfconsole
```

Once inside, I explored some basic commands to understand the framework's structure:

```bash
show exploits    # lists all available exploit modules
show auxiliary   # lists scanner, fuzzer, and DoS modules
show payloads    # lists code that can be executed post-exploitation
show options     # lists configurable options for the current module
```

**Finding the Target IP**

Inside the Metasploitable 2 VM, I ran:

```bash
ifconfig
```

The IP address was listed under `inet addr` on the `eth0` interface under `192.168.x.x`. This is the address I used as the target for the exploit.

**Exploiting VSFTPD v2.3.4 Backdoor**

Metasploitable 2 runs **vsftpd 2.3.4** on port 21 (FTP). This version contains a backdoor that was maliciously inserted into the source code — if a username ending in `:)` is sent to the server, it opens a root shell on port 6200.

I searched for the relevant exploit module:

```bash
search vsftpd
```

This returned `exploit/unix/ftp/vsftpd_234_backdoor`. I selected it and checked its options:

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
show options
```

The only required field not automatically populated was `RHOSTS` (the target IP). I set it and ran the exploit:

```bash
set RHOST 192.168.x.x
run
```

The exploit succeeded — Metasploit reported that the backdoor service had been spawned and a command shell session was opened on port 6200 with `uid=0(root)`, meaning full root access to the Metasploitable 2 machine.

I confirmed access by running basic commands inside the shell:

```bash
pwd   # returned /
ls    # listed the root filesystem
```

> **Note:** During this workshop I encountered an error — `One or more options failed to validate: RHOSTS` — caused by accidentally setting `CHOST` instead of `RHOST`. I cleared the incorrect value with `set CHOST` (no argument) and restarted the process from scratch, which resolved the issue.

---

#### What I Learned

This workshop demonstrated how quickly a known, unpatched vulnerability can be exploited using an automated framework like Metasploit. The VSFTPD backdoor is a real historical vulnerability — it was present in a widely-downloaded FTP server in 2011 before being discovered and patched.

A few key takeaways:

- Metasploit drastically lowers the barrier to exploitation by packaging known vulnerabilities into ready-to-use modules. This is exactly why patch management matters — leaving known vulnerabilities unaddressed gives attackers a straightforward path in.
- The backdoor required no credentials or brute-forcing. A single crafted username was enough to open a root shell — illustrating how supply chain attacks (malicious code inserted into a legitimate software project) can be extremely impactful.
- Network isolation is critical when running intentionally vulnerable systems. Both VMs were kept on a Host-Only adapter throughout to prevent any real-world exposure.

In practice, a penetration tester would first use a scanner (such as Nmap or Metasploit's `db_nmap`) to discover open ports and service versions on a target, then look up known CVEs for those versions before selecting an appropriate exploit.

---

#### OWASP Reference

- **A06:2021 – Vulnerable and Outdated Components**
