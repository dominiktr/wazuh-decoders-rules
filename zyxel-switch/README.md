# Zyxel Switch — Wazuh Decoders & Rules

Wazuh detection content for **Zyxel XS3800** and compatible Zyxel managed switches.

**Tested on:** Zyxel XS3800  
**Rule ID range:** 100300–100399

---

## Files

| File | Description |
|------|-------------|
| `decoders/0600-zyxel-switch_decoders.xml` | Log decoders for all supported facilities |
| `rules/0600-zyxel-switch_rules.xml` | Detection rules with compliance mappings |

> **Note:** Coverage is based on logs observed during real-world testing on the listed hardware.
> Some log variants may not be covered by these decoders.

---

## Supported Facilities

| Facility | Description |
|----------|-------------|
| `authentication` | Login, logout, auth failures |
| `interface` | Link up/down, autonegotiation |
| `system` | NTP, reboot, PSU, firmware, config save |
| `ddmi` | SFP/transceiver diagnostics |
| `dhcp` | Duplicated subnet detection |

---

## Log Format

Zyxel sends syslog in the following format:

```
YYYY-MM-DDTHH:MM:SS+HH:MM HOST facility: message
```

As seen in Wazuh `archives.log`:

```
YYYY Mon DD HH:MM:SS wazuh->SW-IP  YYYY-MM-DDTHH:MM:SS+HH:MM HOST facility: message
```

---

## Sample Logs

### Authentication — successful login
```
2026-02-19T08:02:20+02:00 ZYXEL-SW-01 authentication: SSH user admin login [IP address = 192.168.0.15]
```
→ Decoder: `zyxel-auth-success` | Rule: `100300` (level 3)

### Authentication — failure
```
2026-02-19T08:05:11+02:00 ZYXEL-SW-01 authentication: SSH authentication failure [username: admin, IP address = 192.168.0.99]
```
→ Decoder: `zyxel-auth-fail` | Rule: `100310` (level 5)

### Brute force (auto-triggered)
5+ auth failures from the same IP within 120 seconds.  
→ Rule: `100311` (level 10)

### Interface — link down
```
2026-02-19T09:14:33+02:00 ZYXEL-SW-01 interface: Port 3 link down
```
→ Decoder: `zyxel-interface-link-down` | Rule: `100341` (level 5)

### Interface — link up
```
2026-02-19T09:15:02+02:00 ZYXEL-SW-01 interface: Port 3 link up 1000M
```
→ Decoder: `zyxel-interface-link-up` | Rule: `100340` (level 3)

### System — unexpected reset
```
2026-02-19T07:00:00+02:00 ZYXEL-SW-01 system: System has reset without management command
```
→ Decoder: `zyxel-system-reset-unmanaged` | Rule: `100333` (level 12)

### PSU — state change to failure
```
2026-02-19T11:30:00+02:00 ZYXEL-SW-01 system: The status of the power source PWR1 has changed from 'Normal' to 'Failure' state
```
→ Decoder: `zyxel-system-psu-change` | Rule: `100323` (level 12)

### DDMI — critical alarm
```
2026-02-19T12:00:00+02:00 ZYXEL-SW-01 ddmi: High RxPower Alarm Threshold(3.0dBm),Port 1, CurValue:4.2dBm
```
→ Decoder: `zyxel-ddmi-alarm` | Rule: `100352` (level 12)

### DHCP — duplicated subnet
```
2026-02-19T13:00:00+02:00 ZYXEL-SW-01 dhcp: Duplicated subnet configuration on interface VLAN 10, IP 10.0.0.1, DHCP server 10.0.0.2
```
→ Decoder: `zyxel-dhcp-duplicated-subnet` | Rule: `100360` (level 8)

---

## Rule Summary

| Rule ID | Level | Description |
|---------|-------|-------------|
| 100300 | 3 | Successful login |
| 100301 | 3 | Logout |
| 100302 | 3 | Session connected |
| 100310 | 5 | Authentication failure |
| 100311 | 10 | Brute force — same source IP |
| 100312 | 11 | Brute force — multiple users/protocols |
| 100320 | 3 | NTP sync successful |
| 100321 | 4 | NTP sync failed |
| 100322 | 5 | PSU state change |
| 100323 | 12 | PSU failure/degraded state |
| 100330 | 5 | System cold start |
| 100331 | 5 | System warm start |
| 100332 | 5 | System reboot |
| 100333 | 12 | Unexpected reset (no mgmt command) |
| 100334 | 3 | Reset via management command |
| 100335 | 3 | Firmware boot info |
| 100336 | 8 | Configuration saved |
| 100340 | 3 | Port link up |
| 100341 | 5 | Port link down |
| 100342 | 7 | Port flapping |
| 100343 | 6 | Autonegotiation failed |
| 100344 | 3 | Autonegotiation recovered |
| 100350 | 3 | DDMI alarm cleared |
| 100351 | 7 | DDMI warning threshold exceeded |
| 100352 | 12 | DDMI critical alarm |
| 100360 | 8 | Duplicated DHCP subnet |
| 100370–100374 | 3 | Fallback rules (unrecognized logs) |

---

## Compliance Mappings

Rules include tags for: `PCI DSS`, `GDPR`, `HIPAA`, `NIST 800-53`, `GPG13`, `TSC`.