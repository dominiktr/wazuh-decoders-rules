# Cisco Catalyst 1200 Series — Wazuh Decoders & Rules

Wazuh detection content for **Cisco Catalyst 1200 Series** managed switches.

**Tested on:** Cisco Catalyst 1200  
**Rule ID range:** 1600–1699

---

## Files

| File | Description |
|------|-------------|
| `decoders/0605-Cisco-Catalyst-1200_decoders.xml` | Log decoders for all supported facilities |
| `rules/1600-Cisco-Catalyst-1200_rules.xml` | Detection rules |

> **Note:** Coverage is based on logs observed during real-world testing on the listed hardware.
> Some log variants may not be covered by these decoders.

---

## Supported Facilities

| Facility | Description |
|----------|-------------|
| `AAA` | Authentication, session open/close/reject |
| `COPY` | File copy operations |
| `CBD` | CBD server connection status |
| `INIT` | System initialization and startup |
| `LINK` | Interface up/down, Err-Disabled |
| `STP` | Spanning Tree Protocol status |
| `SNMP` | SNMP auth failures |
| `SSHD` | SSH disconnect events |
| `SSL` | Certificate installation and errors |
| `SYSLOG` | Syslog server add/remove |
| `TIMEMANAGE` | Clock, NTP, RTC, timezone updates |
| `QOS_CLI` | IP ACL changes |
| `SecureBoot` | Secure boot events |
| `MLDP` | MLDP events |
| `Entity` | Hardware/entity events |

---

## Log Format

Cisco Catalyst 1200 sends syslog in the following format:

```
%FACILITY-SEV-MNEMONIC: message
```

As seen in Wazuh `archives.log`:

```
YYYY Mon DD HH:MM:SS agent->device_ip %FACILITY-SEV-MNEMONIC: message
```

---

## Sample Logs

### AAA — new HTTPS session
```
2026-02-24 07:23:41 wazuh->192.168.0.20 %AAA-I-CONNECT: New https connection for user admin, source 192.168.0.15 destination 192.168.0.111 ACCEPTED
```
→ Decoder: `cisco-switch-aaa-connect-https` | Rule: `1600` (level 3)

### AAA — HTTPS session terminated
```
2026-02-24 07:23:41 wazuh->192.168.0.20 %AAA-I-DISCONNECT: https connection for user admin, source 192.168.0.15 destination 192.168.0.111 TERMINATED
```
→ Decoder: `cisco-switch-aaa-disconnect-https` | Rule: `1603` (level 3)

### AAA — SSH connection rejected
```
2026-02-24 08:10:05 wazuh->192.168.0.20 %AAA-W-REJECT: New ssh connection for user admin, source 192.168.0.99 destination 192.168.0.20 REJECTED
```
→ Decoder: `cisco-switch-aaa-reject-ssh` | Rule: `1605` (level 6)

### AAA — SSH CLI session opened
```
2026-02-24 08:15:00 wazuh->192.168.0.20 %AAA-I-CONNECT: User CLI session for user admin over SSH, source 192.168.0.15 destination 192.168.0.20 ACCEPTED
```
→ Decoder: `cisco-switch-aaa-connect-ssh` | Rule: `1602` (level 3)

### LINK — interface down
```
2026-02-24 09:00:00 wazuh->192.168.0.20 %LINK-W-Down: GigabitEthernet5
```
→ Decoder: `cisco-switch-link-down` | Rule: `1626` (level 5)

### LINK — Err-Disabled
```
2026-02-24 09:01:00 wazuh->192.168.0.20 %LINK-W-Err-Disabled: GigabitEthernet5
```
→ Decoder: `cisco-switch-link-generic` | Rule: `1628` (level 8)

### SNMP — unauthorized access
```
2026-02-24 10:00:00 wazuh->192.168.0.20 %SNMP-W-SNMPAUTHFAIL: Access attempted by unauthorized NMS: 10.0.0.99
```
→ Decoder: `cisco-switch-snmp-authfail` | Rule: `1635` (level 8)

### SYSLOG — server removed
```
2026-02-24 11:00:00 wazuh->192.168.0.20 %SYSLOG-I-NOSYSLOGSERVER: delete syslog server 192.168.0.200
```
→ Decoder: `cisco-switch-syslog-nosrv` | Rule: `1651` (level 11)

### QOS — ACL deleted
```
2026-02-24 12:00:00 wazuh->192.168.0.20 %QOS_CLI-I-NETACLCHANGED: IP ACL 'BLOCK_MGMT' has been deleted
```
→ Decoder: `cisco-switch-qos-acl` | Rule: `1666` (level 9)

### SecureBoot — error
```
2026-02-24 13:00:00 wazuh->192.168.0.20 %SecureBoot-E-FAILURE: Image signature verification failed
```
→ Decoder: `cisco-switch-secureboot` | Rule: `1671` (level 12)

---

## Rule Summary

| Rule ID | Level | Description |
|---------|-------|-------------|
| 1600 | 3 | New HTTPS session (authenticated) |
| 1601 | 3 | New HTTPS connection (pre-auth) |
| 1602 | 3 | New SSH CLI session |
| 1603 | 3 | HTTPS session terminated |
| 1604 | 3 | SSH CLI session terminated |
| 1605 | 6 | SSH connection rejected |
| 1606 | 6 | HTTPS connection rejected |
| 1607 | 6 | AAA connection rejected |
| 1608 | 10 | Brute force — multiple SSH rejections |
| 1609 | 2 | AAA generic fallback |
| 1610 | 5 | File copy operation |
| 1611 | 5 | Copy TRAP notification |
| 1615 | 3 | CBD status change |
| 1620 | 3 | Initialization completed |
| 1621 | 7 | Cold startup |
| 1622 | 3 | INIT generic |
| 1625 | 3 | Interface up |
| 1626 | 5 | Interface down |
| 1627 | 4 | LINK generic |
| 1628 | 8 | Interface Err-Disabled |
| 1630 | 4 | STP port status change |
| 1631 | 4 | STP port Blocking |
| 1632 | 4 | STP generic |
| 1635 | 8 | SNMP unauthorized NMS access |
| 1636 | 10 | Multiple SNMP auth failures |
| 1637 | 2 | SNMP CDB items loaded |
| 1640 | 3 | SSH disconnect |
| 1641 | 6 | SSH auth failure |
| 1642 | 6 | SSH no supported auth methods |
| 1645 | 8 | Root CA certificate installed |
| 1647 | 5 | SSL handshake error |
| 1648 | 7 | SSL certificate expired |
| 1649 | 8 | SSL certificate invalid |
| 1650 | 8 | New syslog server configured |
| 1651 | 11 | Syslog server removed |
| 1655 | 5 | System clock updated |
| 1656 | 5 | Time zone updated |
| 1657 | 3 | DST/summer-time updated |
| 1658 | 5 | Clock source changed |
| 1659 | 3 | Clock source list updated |
| 1660 | 3 | RTC clock updated |
| 1665 | 8 | IP ACL modified |
| 1666 | 9 | IP ACL deleted |
| 1670 | 5 | SecureBoot event |
| 1671 | 12 | SecureBoot ERROR |
| 1675 | 3 | MLDP event |
| 1680 | 3 | Entity event |
| 1681 | 7 | Entity ERROR |