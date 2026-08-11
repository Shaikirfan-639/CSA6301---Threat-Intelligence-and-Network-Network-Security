def ips_process(packet, rules, whitelist=None):
    """
    Like scan_packet, but decides whether to BLOCK (untrusted source) or
    ALLOW-WITH-LOG (source is on the tuning whitelist, e.g. a known partner
    IP that reliably triggers a benign false positive).
    """
    whitelist = whitelist or set()

    alerts = scan_packet(packet, rules)

    if not alerts:
        return {
            "action": "allow",
            "alerts": []
        }

    if packet.get("src_ip") in whitelist:
        return {
            "action": "allow",
            "alerts": alerts,
            "note": "source whitelisted, alert suppressed from blocking"
        }

    return {
        "action": "block",
        "alerts": alerts
    }


    def test_experiment4():
    attack_packet = {
        "proto": "tcp",
        "dst_port": 80,
        "payload": "union select username,password from users",
        "src_ip": "203.0.113.50"
    }

    result = ips_process(attack_packet, IDS_RULES)

    assert result["action"] == "block"

    # Identical signature match, but from a whitelisted partner IP
    partner_packet = dict(
        attack_packet,
        src_ip="198.51.100.10"
    )

    result2 = ips_process(
        partner_packet,
        IDS_RULES,
        whitelist={"198.51.100.10"}
    )

    assert result2["action"] == "allow"
    assert "note" in result2

    print("All test cases passed.")


test_experiment4()






![Uploading image.png…]()
