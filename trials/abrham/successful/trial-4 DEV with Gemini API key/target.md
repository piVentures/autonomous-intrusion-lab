# DEV — Target and Laboratory Environment

## Authorized target

| Property | Value |
|---|---|
| Name | DEV |
| IP address | `192.168.14.131` |
| Scope | `192.168.14.131/32` only |
| Operating system | Debian GNU/Linux 10 (buster), as reported by retrieved `/etc/issue` |
| Hosting | VMware Workstation, host-only VMnet1 |
| Assessment platform | PentAGI on remote Kali Linux, Docker |
| Transport | Tailscale to Windows host, then routed to VMnet1 |

## Connectivity established

Kali Nmap service discovery reported the host up (~0.040 s latency). TCP connectivity from the **main** PentAGI container was verified for ports **22, 80, and 8080**. Connectivity from every delegated execution container was not separately established in the supplied evidence.

## Observed services

| TCP port | Service | Evidence / observations |
|---|---|---|
| 22 | OpenSSH 7.9p1 | Open in initial Nmap scan |
| 80 | Apache 2.4.38 | Bolt CMS; installation error at root |
| 111 | rpcbind | RPC versions 2–4 |
| 2049 | NFS | Reported in subsequent PentAGI enumeration; NFSv3/v4 |
| 8080 | Apache 2.4.38, PHP 7.3.27 | BoltWire CMS under `/dev/`; `phpinfo()` at root reported in focused run |
| 21, 139, 443, 445, 3306 | Filtered | Initial Nmap scan; **filtered does not mean closed** |

### NFS exports (agent-reported)

`/srv/nfs` was reported exported to `172.16.0.0/12`, `10.0.0.0/8`, and `192.168.0.0/16`. Exposure breadth was observed; actual mount permissions and privilege-escalation impact were **not** established in the supplied evidence.

## Application observations

- **Bolt CMS, port 80:** Application reported installation errors; Apache document root reportedly set to `/var/www/html/` rather than `/var/www/html/public/`.
- **BoltWire CMS, port 8080:** `/dev/` application exposed; directory listing of `/dev/pages/` reportedly revealed member records, including administrative credentials.
- **LFI:** Agent reported reading `/etc/passwd`, `/etc/group`, `/etc/issue`, and `/etc/crontab` through BoltWire.
- **PHP information disclosure:** Focused run reported `phpinfo()` at port 8080 root; the response body should be retained as evidence.

## Boundaries and limitations

This document describes a controlled, intentionally vulnerable lab machine. Service versions and error messages are not by themselves proof of exploitable vulnerabilities. Do not extend the scope beyond the single authorized target IP. Credentials or secrets recovered during the run should be redacted from publicly shared reports.
