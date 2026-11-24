# unycfd

<p align="center">
  <b>Ultra-lightweight, memory-efficient Cloudflare Tunnel infrastructure in pure Rust.</b><br>
  <i>Drop-in replacement for Go-based <code>cloudflared</code>. &lt; 3 MB RSS. Native QUIC.</i>
</p>

<p align="center">
  <a href="https://github.com/unycfd/unycfd/releases"><img src="https://img.shields.io/github/v/release/unycfd/unycfd?include_prereleases&style=flat-square&color=blue" alt="Latest Release"></a>
  <a href="https://github.com/unycfd/unycfd/actions"><img src="https://img.shields.io/github/actions/workflow/status/unycfd/unycfd/ci.yml?branch=main&style=flat-square" alt="CI Status"></a>
  <a href="https://github.com/unycfd/unycfd/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT%2FApache--2.0-blue?style=flat-square" alt="License"></a>
  <img src="https://img.shields.io/badge/memory-&lt;%203%20MB%20RSS-brightgreen?style=flat-square" alt="Memory Footprint">
</p>

---

## ⚡ Why unycfd?

The official `cloudflared` daemon is a capable tool, but its Go runtime and buffer defaults carry a **15–35 MB RSS** memory footprint and a 40–60 MB binary. 

On single-purpose edge nodes, IoT devices, micro-VMs, Kubernetes clusters, or multi-tenant host systems running dozens of tunnels in parallel (e.g. 20 tunnels = ~600 MB RAM consumed just for ingress routing), this overhead adds up fast.

**unycfd reimagines Cloudflare Tunnels from first principles in Rust:**
- **~2.8 – 3.6 MB RSS** per tunnel instance — up to **10× less memory**.
- **Native QUIC Transport (`quinn`)** with 0-RTT/1-RTT handshakes and automatic HTTP/2 fallback.
- **Drop-in Configuration Compatibility** — supports standard Cloudflare credentials JSON, connector tokens (`--token`), and multi-host `ingress` YAML.
- **Systemd `Type=notify`** — zero-downtime, verified edge handshakes before reporting ready.

---

## 📊 At a Glance

| Metric / Feature | Official `cloudflared` | `unycfd` |
| :--- | :--- | :--- |
| **Language** | Go | **Pure Rust** |
| **Runtime Memory (RSS)** | ~15 – 35 MB | **~2.8 – 3.6 MB** |
| **Binary Size** | ~40 – 60 MB | **~3.3 MB** (stripped musl) |
| **Edge Protocol** | QUIC / HTTP2 | **QUIC (`quinn`) + H2 Fallback** |
| **Named Tunnels** | ✅ Yes | ✅ **Yes** (JSON & Token) |
| **Socket Support** | TCP / Unix socket | **TCP / Unix socket (`unix:/path.sock`)** |
| **Systemd Notification** | Periodic logs | **Native `sd_notify` (`READY=1`)** |
| **Quick Tunnels** | ✅ `trycloudflare.com` | ✅ `trycloudflare.com` |

---

## 📦 Projects & Crates

The `unycfd` ecosystem provides both ready-to-run binaries and reusable Rust libraries:

- **[`unycfd`](https://github.com/unycfd/unycfd)**  
  The main CLI daemon and drop-in service replacement for `cloudflared`. Features multi-tunnel systemd template units (`unycfd@.service`), ingress routing, and WebSocket upgrades.
- **[`libcfd`](https://github.com/unycfd/unycfd/tree/main/crates/libcfd)**  
  Modular, embeddable Rust client library for Cloudflare edge RPC and QUIC/H2 tunnels. Embed tunnel connectors directly into your own Rust services without external daemon processes.
- **[`libcfd-rpc`](https://github.com/unycfd/unycfd/tree/main/crates/libcfd-rpc)**  
  Cap'n Proto RPC framing layer tuned with `Send` futures for modern async runtimes.

---

## 🚀 Quick Start

### 1. Download Pre-built Binary
Fetch the latest standalone release binary for Linux (x86_64, aarch64), macOS, or Windows from [Releases](https://github.com/unycfd/unycfd/releases).

```bash
# Example for Linux x86_64
curl -L -o unycfd https://github.com/unycfd/unycfd/releases/latest/download/unycfd-linux-x86_64
chmod +x unycfd
sudo mv unycfd /usr/local/bin/
```

### 2. Run Instantly

```bash
# Ephemeral Quick Tunnel (trycloudflare.com)
unycfd --url http://localhost:8080

# Production Named Tunnel via Zero Trust Token
unycfd --token <CF_TUNNEL_TOKEN>

# Production Named Tunnel via Configuration File
unycfd --config /etc/cloudflared/config.yml
```

---

## 🎯 Primary Use Cases

- **Density-Constrained Hosting**: Run 50+ isolated tunnel instances on a 1 GB RAM VPS without pressure on memory limits.
- **Embedded & Edge (Routers, Raspberry Pi, IoT)**: Seamless zero-overhead ingress for memory-sensitive devices.
- **Kubernetes Sidecars**: Drastically reduce container memory requests and cluster node overcommit.
- **OpenResty / Nginx Ingress**: Direct routing through Unix domain sockets (`unix:/run/openresty/nginx.sock`).

---

<p align="center">
  <sub>Maintained by the <b>unycfd</b> team. Contributions and feedback are welcome!</sub>
</p>
