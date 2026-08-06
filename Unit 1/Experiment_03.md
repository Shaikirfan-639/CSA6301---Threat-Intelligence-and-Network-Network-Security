import socket

def scan_port(host, port):
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  s.settimeout(0.5)
  result = s.connect_ex((host, port))
  s.close()
  return result == 0

target = "127.0.0.1" # scan your own machine (localhost)
ports = [21, 22, 23, 25, 80, 443, 3306, 8080]
print(f"Scanning {target} ...")
for port in ports:
  status = "OPEN" if scan_port(target, port) else "closed"
  print(f"Port {port}: {status}")
  
  
  
  <img width="883" height="871" alt="Screenshot 2026-07-11 140544" src="https://github.com/user-attachments/assets/4321224f-8754-4746-a33d-cf86539e107d" />
