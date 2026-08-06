import hashlib

sample_file_content = (
    b"MZ\x90\x00..."
    b"fake_pe_header..."
    b"CreateRemoteThread..."
    b"WriteProcessMemory..."
)

suspicious_api_keywords = [
    b"CreateRemoteThread",
    b"WriteProcessMemory",
    b"RegSetValue",
    b"InternetOpenUrl",
]


def static_analysis(file_bytes):
    file_hash = hashlib.sha256(file_bytes).hexdigest()

    found_apis = [
        api.decode()
        for api in suspicious_api_keywords
        if api in file_bytes
    ]

    return {
        "sha256": file_hash,
        "suspicious_apis_found": found_apis,
        "static_risk_score": len(found_apis),
    }


def dynamic_analysis(sandbox_log):
    indicators = 0
    reasons = []

    if sandbox_log.get("registry_writes", 0) > 0:
        indicators += 1
        reasons.append("Modified registry")

    if sandbox_log.get("network_connections", 0) > 0:
        indicators += 1
        reasons.append("Made outbound network connection")

    if sandbox_log.get("new_processes_spawned", 0) > 1:
        indicators += 1
        reasons.append("Spawned multiple child processes")

    return {
        "dynamic_risk_score": indicators,
        "reasons": reasons,
    }


def classify(static_result, dynamic_result, threshold=2):
    total = (
        static_result["static_risk_score"]
        + dynamic_result["dynamic_risk_score"]
    )

    return "malicious" if total >= threshold else "benign"


static_result = static_analysis(sample_file_content)

sandbox_log = {
    "registry_writes": 3,
    "network_connections": 1,
    "new_processes_spawned": 2,
}

dynamic_result = dynamic_analysis(sandbox_log)

verdict = classify(static_result, dynamic_result)

benign_static = static_analysis(
    b"just a normal text file with no suspicious content"
)

benign_dynamic = dynamic_analysis(
    {
        "registry_writes": 0,
        "network_connections": 0,
        "new_processes_spawned": 0,
    }
)

benign_verdict = classify(benign_static, benign_dynamic)

print("Static Analysis:")
print(static_result)

print("\nDynamic Analysis:")
print(dynamic_result)

print("\nVerdict:")
print(verdict)

print("\nBenign File Verdict:")
print(benign_verdict)




<img width="1782" height="787" alt="image" src="https://github.com/user-attachments/assets/c833f19a-1ca7-49d2-b217-ac6e26962910" />
