````markdown
# Lab 3 — Network Troubleshooting & DNS

## Objective
Practice basic network troubleshooting using `ipconfig`, `ping`, `nslookup`, `tracert`, and DNS cache commands.

## 1. Network Configuration

Command:

```cmd
ipconfig /all
````

Observed configuration:

* IPv4: `192.168.220.2`
* Subnet Mask: `255.255.255.0`
* Default Gateway: `192.168.220.254`
* DNS Server: `192.168.220.2`
* DNS Suffix: `lab.local`

This confirmed that the Server was connected to the `192.168.220.0/24` network.

## 2. Gateway / Network Test

Tested:

```cmd
ping 192.168.220.2
```

Result:

* 4 packets sent
* 4 received
* 0% packet loss

This confirmed IP connectivity to the target.

## 3. Incorrect Network Test

Tested:

```cmd
ping 192.168.100.10
```

Result:

```text
Request timed out.
100% loss
```

### Cause

The tested address belonged to a different network than the Server's current `192.168.220.x` configuration.

### Lesson

Always check the current IP/subnet before troubleshooting connectivity.

## 4. DNS Troubleshooting

Tested:

```cmd
nslookup lab.local
```

and:

```cmd
nslookup win-20714e5btar.lab.local
```

Result:

```text
*** Unknown can't find lab.local: Non-existent domain
```

### Cause

The DNS server did not contain DNS records for `lab.local` or the computer hostname.

### Lesson

Successful IP connectivity does not guarantee successful DNS resolution.

## 5. Hostname vs IP Test

Tested:

```cmd
ping 192.168.100.10
```

and:

```cmd
ping win-20714e5btar.lab.local
```

The IP test reached the network layer but the hostname could not be resolved.

This demonstrates the difference between:

* **IP connectivity**
* **DNS name resolution**

## 6. Routing Test

Command:

```cmd
tracert 192.168.220.2
```

Result:

```text
1    <1 ms    <1 ms    <1 ms    192.168.220.2
Trace complete.
```

Only one hop appeared because the destination was on the same local network.

## 7. DNS Cache Refresh

Commands:

```cmd
ipconfig /flushdns
ipconfig /registerdns
ipconfig /renew
```

Results:

* DNS Resolver Cache successfully flushed.
* DNS registration was initiated.
* DHCP renewal was attempted.

## Troubleshooting Summary

| Test                    | Result     | Finding                        |
| ----------------------- | ---------- | ------------------------------ |
| `ipconfig /all`         | Successful | Identified IP, gateway and DNS |
| `ping 192.168.220.2`    | Successful | IP connectivity works          |
| `ping 192.168.100.10`   | Failed     | Wrong/different subnet         |
| `nslookup lab.local`    | Failed     | DNS record/domain unavailable  |
| `nslookup hostname`     | Failed     | Host DNS record unavailable    |
| `tracert`               | Successful | Destination is local           |
| `ipconfig /flushdns`    | Successful | DNS cache cleared              |
| `ipconfig /registerdns` | Successful | DNS registration initiated     |

## Key Takeaway

The main issue discovered was not basic network connectivity, but **DNS/name resolution and incorrect network addressing**. Proper troubleshooting starts with verifying the IP configuration, then testing connectivity, and finally testing DNS resolution.

```
```
