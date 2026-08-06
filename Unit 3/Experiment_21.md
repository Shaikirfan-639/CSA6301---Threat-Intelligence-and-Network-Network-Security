import statistics

# Hypothesis-based: "attackers may use encoded PowerShell commands"

execution_logs = [
    {
        "process": "powershell.exe",
        "args": "-enc SGVsbG8=",
        "user": "svc_admin",
    },
    {
        "process": "notepad.exe",
        "args": "",
        "user": "jdoe",
    },
    {
        "process": "powershell.exe",
        "args": "-nop -w hidden -enc ZXZpbA==",
        "user": "jdoe",
    },
]


def hypothesis_hunt_powershell(logs):
    return [
        l
        for l in logs
        if l["process"] == "powershell.exe" and "-enc" in l["args"]
    ]


# Intelligence-based: match connections against a known-malicious-IP feed

connection_logs = [
    {"ip": "45.33.32.156"},
    {"ip": "8.8.8.8"},
    {"ip": "185.220.101.1"},
]

known_malicious_ips = {
    "45.33.32.156",
    "185.220.101.1",
}


def intelligence_hunt(logs, feed):
    return [l for l in logs if l["ip"] in feed]


# Analytics-based: flag a login hour that deviates far from the user's normal
# pattern

login_hours_history = [9, 9, 10, 9, 8, 9, 10]
new_login_hour = 3


def analytics_hunt(history, new_value, threshold=2.0):
    mean = statistics.mean(history)
    stdev = statistics.pstdev(history) or 1
    z = abs(new_value - mean) / stdev

    return {
        "z_score": round(z, 2),
        "anomalous": z > threshold,
    }


hyp_result = hypothesis_hunt_powershell(execution_logs)
intel_result = intelligence_hunt(connection_logs, known_malicious_ips)
analytics_result = analytics_hunt(login_hours_history, new_login_hour)

print("Hypothesis-based hits:", hyp_result)
print("Intelligence-based hits:", intel_result)
print("Analytics-based result:", analytics_result)

<img width="1917" height="660" alt="image" src="https://github.com/user-attachments/assets/32e04cfb-9649-4992-9122-c71c79154121" />





