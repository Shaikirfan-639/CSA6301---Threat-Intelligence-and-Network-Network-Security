import re

def detect_phishing(url):
    score = 0

    # Check if URL uses HTTPS
    if not url.startswith("https://"):
        score += 1

    # Check for IP address instead of domain name
    ip_pattern = r'https?://(?:\d{1,3}\.){3}\d{1,3}'
    if re.match(ip_pattern, url):
        score += 2

    # Check for '@' symbol
    if '@' in url:
        score += 2

    # Check for excessive URL length
    if len(url) > 75:
        score += 1

    # Check for multiple hyphens
    if url.count('-') >= 2:
        score += 1

    # Check for suspicious keywords
    suspicious_words = [
        "login", "verify", "secure", "account",
        "update", "bank", "paypal", "signin"
    ]

    for word in suspicious_words:
        if word in url.lower():
            score += 1

    print("\nURL:", url)
    print("Risk Score:", score)

    if score >= 4:
        print("⚠️ Phishing URL Detected")
    else:
        print("✅ Legitimate URL")

url = input("Enter URL: ")
detect_phishing(url)


<img width="752" height="452" alt="image" src="https://github.com/user-attachments/assets/c8e1689b-4f95-44cd-a41b-a03980683464" />





