splunk_index = [
    {
        "_time": "09:15",
        "source": "auth",
        "user": "jdoe",
        "action": "login_success",
    },
    {
        "_time": "09:17",
        "source": "auth",
        "user": "jdoe",
        "action": "login_failed",
    },
    {
        "_time": "09:18",
        "source": "auth",
        "user": "jdoe",
        "action": "login_failed",
    },
    {
        "_time": "09:20",
        "source": "firewall",
        "ip": "45.33.32.156",
        "action": "blocked",
    },
    {
        "_time": "09:21",
        "source": "auth",
        "user": "asmith",
        "action": "login_success",
    },
]


def spl_search(index, **filters):
    results = index

    for field, value in filters.items():
        results = [
            r
            for r in results
            if r.get(field) == value
        ]

    return results


def spl_stats_count_by(index, field):
    counts = {}

    for r in index:
        key = r.get(field, "unknown")
        counts[key] = counts.get(key, 0) + 1

    return counts


failed_logins = spl_search(
    splunk_index,
    source="auth",
    action="login_failed",
)

counts_by_source = spl_stats_count_by(
    splunk_index,
    "source",
)

print("Search Results:")
for log in failed_logins:
    print(log)

print("\nStatistics Count by Source:")
for source, count in counts_by_source.items():
    print(f"{source}: {count}")






    <img width="1000" height="703" alt="image" src="https://github.com/user-attachments/assets/f6cd755c-4b9f-4b32-8899-0d08acfe101c" />
