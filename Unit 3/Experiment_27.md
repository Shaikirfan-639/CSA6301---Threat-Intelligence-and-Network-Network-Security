import re

raw_log_lines = [
    "2026-07-25 09:15:01 INFO auth: user=jdoe action=login_success ip=10.0.0.5",
    "2026-07-25 09:17:32 WARN auth: user=jdoe action=login_failed ip=10.0.0.5",
    "2026-07-25 09:20:10 ERROR firewall: action=blocked ip=45.33.32.156",
]


def logstash_parse(line):
    pattern = (
        r"(?P<date>\S+) "
        r"(?P<time>\S+) "
        r"(?P<level>\w+) "
        r"(?P<source>\w+): "
        r"(?P<fields>.*)"
    )

    m = re.match(pattern, line)

    if not m:
        return None

    doc = m.groupdict()
    field_str = doc.pop("fields")

    for kv in field_str.split():
        if "=" in kv:
            k, v = kv.split("=", 1)
            doc[k] = v

    return doc


elasticsearch_index = [
    logstash_parse(line)
    for line in raw_log_lines
]


def es_search(index, **query):
    return [
        doc
        for doc in index
        if all(doc.get(k) == v for k, v in query.items())
    ]


def kibana_aggregate(index, field):
    agg = {}

    for doc in index:
        key = doc.get(field, "unknown")
        agg[key] = agg.get(key, 0) + 1

    return agg


parsed_ok = all(d is not None for d in elasticsearch_index)

failed_search = es_search(
    elasticsearch_index,
    action="login_failed",
)

level_aggregation = kibana_aggregate(
    elasticsearch_index,
    "level",
)

print("Parsed documents:")
for d in elasticsearch_index:
    print(d)

print("\nSearch Results (action=login_failed):")
for d in failed_search:
    print(d)

print("\nKibana Aggregation by Level:")
for level, count in level_aggregation.items():
    print(f"{level}: {count}")





    <img width="1401" height="781" alt="image" src="https://github.com/user-attachments/assets/4cdafc36-30b8-457b-ae6a-0e24fc966770" />
