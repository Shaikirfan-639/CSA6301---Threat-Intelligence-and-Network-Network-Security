auth_log = [
    ("09:15", "login_success"),
    ("09:17", "failed_login"),
    ("09:18", "failed_login"),
    ("09:19", "failed_login"),
    ("09:20", "account_locked"),
]


def detect_bruteforce(log, fail_threshold=3):
    consecutive_fails = 0

    for _, event in log:
        if event == "failed_login":
            consecutive_fails += 1

        elif (
            event == "account_locked"
            and consecutive_fails >= fail_threshold
        ):
            return True, consecutive_fails

        else:
            consecutive_fails = 0

    return False, consecutive_fails


def classify_log_line(line):
    keywords = {
        "firewall": ["blocked", "dropped", "denied"],
        "security": ["login", "locked", "authentication"],
        "web_server": ["get", "post", "http"],
    }

    line_lower = line.lower()

    for category, words in keywords.items():
        if any(w in line_lower for w in words):
            return category

    return "unknown"


detected, fails = detect_bruteforce(auth_log)

sample_classification = classify_log_line(
    "User login failed - authentication error"
)

print(
    "Brute-force detected:",
    detected,
    "| consecutive fails before lockout:",
    fails,
)

print("Log line classified as:", sample_classification)

<img width="925" height="771" alt="image" src="https://github.com/user-attachments/assets/dc21b0d0-d583-4328-ab42-9420b5c94223" />



