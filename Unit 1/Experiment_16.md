from collections import Counter

# Sample server log (IP addresses)
logs = [
    "192.168.1.10",
    "192.168.1.20",
    "192.168.1.10",
    "192.168.1.30",
    "192.168.1.10",
    "192.168.1.10",
    "192.168.1.20",
    "192.168.1.10",
    "192.168.1.40",
    "192.168.1.10",
    "192.168.1.50",
    "192.168.1.10",
    "192.168.1.20"
]

# Threshold for DoS detection
THRESHOLD = 5

# Count requests from each IP
request_count = Counter(logs)

print("Request Count Per IP")
print("-" * 30)

for ip, count in request_count.items():
    print(f"{ip} : {count} requests")

print("\nDetection Result")
print("-" * 30)

attack_found = False

for ip, count in request_count.items():
    if count > THRESHOLD:
        print(f"⚠️ Possible DoS Attack Detected from IP: {ip}")
        print(f"Total Requests: {count}\n")
        attack_found = True

if not attack_found:
    print("✅ No DoS Attack Detected")

<img width="907" height="610" alt="image" src="https://github.com/user-attachments/assets/7d9cabea-006c-439c-aef3-087aec319b68" />




