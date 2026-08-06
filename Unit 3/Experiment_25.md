tool_catalog = {
    "Wireshark": {
        "purpose": "packet analysis",
        "keywords": [
            "packet analysis",
            "packet capture",
            "network traffic",
        ],
    },
    "Nmap": {
        "purpose": "network scanning",
        "keywords": [
            "network scanning",
            "open ports",
            "port scan",
        ],
    },
    "Nessus": {
        "purpose": "vulnerability assessment",
        "keywords": [
            "vulnerability assessment",
            "known vulnerabilities",
        ],
    },
    "Burp Suite": {
        "purpose": "web application testing",
        "keywords": [
            "web application testing",
            "http proxy",
        ],
    },
    "Metasploit": {
        "purpose": "penetration testing",
        "keywords": [
            "penetration testing",
            "exploit code",
        ],
    },
    "Splunk": {
        "purpose": "log management and SIEM",
        "keywords": [
            "log search",
            "siem dashboard",
        ],
    },
    "ELK Stack": {
        "purpose": "log collection and visualization",
        "keywords": [
            "log visualization",
            "elasticsearch",
        ],
    },
    "Snort": {
        "purpose": "intrusion detection",
        "keywords": [
            "intrusion detection",
        ],
    },
    "VirusTotal": {
        "purpose": "malware scanning",
        "keywords": [
            "malware scanning",
            "file hash reputation",
        ],
    },
    "MISP": {
        "purpose": "threat intelligence sharing",
        "keywords": [
            "threat intelligence sharing",
            "indicators of compromise",
        ],
    },
}


def recommend_tool(task_description):
    task_lower = task_description.lower()

    scores = {
        tool: sum(
            1
            for kw in info["keywords"]
            if kw in task_lower
        )
        for tool, info in tool_catalog.items()
    }

    scores = {
        tool: score
        for tool, score in scores.items()
        if score > 0
    }

    return max(scores, key=scores.get) if scores else None


test_tasks = [
    "We need to perform network scanning to find open ports on the target.",
    "The analyst wants to inspect packet capture data during the incident.",
    "Check the file hash reputation to see if it's known malware.",
    "Our team wants to enable threat intelligence sharing of indicators of compromise with partners.",
]


recommendations = [
    recommend_tool(task)
    for task in test_tasks
]

print("Recommendations:")

for task, tool in zip(test_tasks, recommendations):
    print(f"Task: {task}")
    print(f"Recommended Tool: {tool}")
    print()


<img width="1173" height="702" alt="image" src="https://github.com/user-attachments/assets/178c8d44-497d-4491-805b-6c5419c3a16d" />

