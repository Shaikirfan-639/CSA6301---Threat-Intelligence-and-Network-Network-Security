def correlate_logs(firewall_log, windows_log, vpn_log):
    indicators = 0
    reasons = []

    if firewall_log.get("event") == "failed_login":
        indicators += 1
        reasons.append("Failed login at firewall")

    if windows_log.get("event") == "account_locked":
        indicators += 1
        reasons.append("Windows account locked")

    if vpn_log.get("failed_attempts", 0) >= 3:
        indicators += 1
        reasons.append("Repeated VPN authentication failures")

    return {
        "indicators": indicators,
        "reasons": reasons,
        "brute_force_detected": indicators >= 2,
    }


scenario_attack = correlate_logs(
    {"event": "failed_login", "ip": "192.168.10.5"},
    {"event": "account_locked", "user": "jdoe"},
    {"failed_attempts": 5},
)

scenario_normal = correlate_logs(
    {"event": "login_success"},
    {"event": "none"},
    {"failed_attempts": 0},
)

print("Attack scenario:", scenario_attack)
print("Normal scenario:", scenario_normal)





<img width="1706" height="612" alt="image" src="https://github.com/user-attachments/assets/32e12d82-67b0-4108-a3c2-0144d0b19f71" />
