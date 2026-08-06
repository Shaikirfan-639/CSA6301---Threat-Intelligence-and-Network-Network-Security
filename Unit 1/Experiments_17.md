from scapy.all import sniff, IP

def packet_callback(packet):
    if packet.haslayer(IP):
        print("Source IP      :", packet[IP].src)
        print("Destination IP :", packet[IP].dst)
        print("Protocol       :", packet[IP].proto)
        print("-" * 40)

print("Capturing 10 packets...\n")

sniff(filter="ip", prn=packet_callback, count=10)


<img width="1920" height="1080" alt="Screenshot 2026-07-25 142628" src="https://github.com/user-attachments/assets/ee4a559d-ae35-4f86-a65f-6844f44b5c11" />
