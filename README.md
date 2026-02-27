# wazuh-decoders-rules

Community-maintained **Wazuh decoders and rules** for network devices - tested on real hardware.

> All detection content in this repository has been developed and validated against physical devices in a production-adjacent environment, not generated from documentation alone.


## Disclaimer

These decoders and rules cover log types observed during testing on the listed hardware.
Not all possible log messages, firmware versions, or device configurations are guaranteed
to be supported. If you encounter an unrecognized log consider opening an issue or submitting
a pull request with a sample log.

Use at your own risk. Always test in a non-production environment before deploying.

---

## Installation

### 1. Copy decoders

```bash
cp <device>/decoders/*.xml /var/ossec/etc/decoders/
```

### 2. Copy rules

```bash
cp <device>/rules/*.xml /var/ossec/etc/rules/
```

### 3. Restart Wazuh manager

```bash
systemctl restart wazuh-manager
```

### 4. Verify

```bash
/var/ossec/bin/wazuh-logtest
```

Paste a sample log from the device and confirm the expected decoder and rule fire correctly.

---

## Compatibility

| Component | Version |
|-----------|---------|
| Wazuh Manager | 4.x |
| Wazuh Agent | 4.x |

---

## License

MIT — see [LICENSE](./LICENSE)
