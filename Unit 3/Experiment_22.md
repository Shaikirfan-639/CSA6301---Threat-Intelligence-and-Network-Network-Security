raw_events = [
    {
        "source": "firewall",
        "ip": "45.33.32.156",
        "event": "blocked",
    },
    {
        "source": "antivirus",
        "host": "WIN10-05",
        "event": "malware_detected",
        "file": "invoice.exe",
    },
    {
        "source": "server",
        "event": "login_success",
        "user": "admin",
    },
]


def normalize(events):
    return [
        {
            "source": e["source"],
            "event_type": e["event"],
            "severity": (
                "high"
                if e["event"] in ("malware_detected", "blocked")
                else "low"
            ),
        }
        for e in events
    ]


def correlation_engine(normalized_events):
    return [
        e
        for e in normalized_events
        if e["severity"] == "high"
    ]


def generate_alerts(high_severity_events):
    return [
        f"ALERT: {e['source']} reported {e['event_type']}"
        for e in high_severity_events
    ]


normalized = normalize(raw_events)
correlated = correlation_engine(normalized)
siem_alerts = generate_alerts(correlated)

print("Normalized:", normalized)
print("Correlated (high severity):", correlated)
print("Alerts:", siem_alerts)



<img width="1917" height="597" alt="image" src="https://github.com/user-attachments/assets/fd3c5948-0e78-4162-a65a-c8e7aa2f4c58" />
