def rule_matches(packet, rule):
    def field_ok(value, rule_value):
        return rule_value == "any" or value == rule_value

    return (
        field_ok(packet["src"], rule["src"])
        and field_ok(packet["dst"], rule["dst"])
        and field_ok(packet["port"], rule["port"])
        and field_ok(packet["proto"], rule["proto"])
    )


def evaluate_packet(packet, rules):
    """
    Return the action of the first matching rule (top-down),
    or 'deny' if no rule matches (default-deny).
    """
    for rule in rules:
        if rule_matches(packet, rule):
            return rule["action"]

    return "deny"



    <img width="991" height="718" alt="image" src="https://github.com/user-attachments/assets/080f787c-2ce0-4ce1-a20f-182bf261a4da" />
