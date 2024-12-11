wififo - OpenWRT daemon for WAN multi-wifi failover

Features
- user list of wifi networks ssid and keys, ordered by priority
- scans wifi APs and connect to the most prioritized
- allow specifying [SQM](https://openwrt.org/docs/guide-user/network/traffic-shaping/sqm) download/upload values per wifi
- continuously tests connectivity (ICMP)
- continuously tests if a higher-priority wifi becomes available
- no extra OpenWRT modifications needed, wififo sets current wifi network via regular `uci` commands

# Install

Copy `wififo` to the target openwrt system:

```
scp wififo root@192.168.1.1:/etc/init.d/
scp wififo.conf.example root@192.168.1.1:/etc/
```

Start and enable `wififo` at boot:

```
chmod +x /etc/init.d/wififo
/etc/init.d/wififo start
/etc/init.d/wififo enable
```

# Configure

The list of wifi networks is provided through `/etc/wififo.conf` with this format:
```
- <ssid>
<psk>
<network_interface> <openwrt_interface_name> <encryption> [<sqm_download> [<sqm_upload>]]
```

The networks must be specified in order of priority.

For example (see `wififo.conf.example`):
```
- BestSSID
ThisIsAComplexWPAKey
wlan0 wifinet1 psk2
- OtherSSID
LittleKeyKey
wlan0 wifinet1 psk2
# IgnoredNetwork
mypskmypsk
wlan0 wifinet1 psk2
```

# Troubleshooting

Logs are in `/tmp/wififo.log`

Start in foreground and log to stdout:
```
/etc/init.d/wififo main
```

Enable verbose logging:
```
VERBOSE=1 /etc/init.d/wififo main
```

# Alternatives

- Use `wpa_supplicant`, but this requires tweaking and does not provide ping connectivity tests.
Support for User-Defined Wpa-Supplicant Config: https://forum.openwrt.org/t/support-for-user-defined-wpa-supplicant-config/2603/8
