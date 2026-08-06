import socket

target = input("Enter Target IP or Website: ")

# Common ports to scan
ports = [20, 21, 22, 23, 25, 53, 80, 110, 143, 443]

print(f"\nScanning Target: {target}")
print("-" * 40)

for port in ports:
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(1)

    result = s.connect_ex((target, port))

    if result == 0:
        print(f"Port {port} : OPEN")
    else:
        print(f"Port {port} : CLOSED")

    s.close()

print("\nScanning Completed.")

<img width="668" height="737" alt="image" src="https://github.com/user-attachments/assets/11c0771a-633a-4663-9d07-875d4d0ece3e" />
