def normalize_ioc(raw):
    return {
        "type": raw.get("type", "").lower(),
        "value": raw.get("value", "").strip().lower(),
        "source": raw.get("source", "unknown"),
        "severity": raw.get("severity", "medium").lower(),
    }


raw_feed = [
    {
        "type": "IP",
        "value": " 45.33.32.156 ",
        "source": "MISP",
        "severity": "High",
    },
    {
        "type": "Domain",
        "value": "Malicious-Site.COM",
        "source": "VirusTotal",
        "severity": "critical",
    },
    {
        "type": "Hash",
        "value": "5d41402abc4b2a76b9719d911017c592",
        "source": "AlienVault OTX",
        "severity": "Medium",
    },
]


tip_database = [normalize_ioc(r) for r in raw_feed]


internal_firewall_logs = [
    {
        "src_ip": "45.33.32.156",
        "dest": "internal-server-1",
        "action": "connection attempt",
    },
    {
        "src_ip": "10.0.0.5",
        "dest": "internal-server-2",
        "action": "connection attempt",
    },
]


def correlate_with_tip(logs, tip_db):
    ioc_ips = {i["value"] for i in tip_db if i["type"] == "ip"}

    return [
        {
            "alert": "Known malicious IP contacted internal system",
            "log": log,
        }
        for log in logs
        if log["src_ip"] in ioc_ips
    ]


alerts = correlate_with_tip(internal_firewall_logs, tip_database)


print(f"Normalized {len(tip_database)} IOCs:")

for ioc in tip_database:
    print(" ", ioc)

print(f"Generated {len(alerts)} alert(s):", alerts)



<img width="1917" height="652" alt="image" src="https://github.com/user-attachments/assets/7bae540c-92c5-45ec-8657-434d7316f9df" />
