import socket
domains = ["www.google.com", "www.python.org", "notarealdomain12345.com"]

for d in domains:
  try:
    ip = socket.gethostbyname(d)
    print(f"{d:30s} -> {ip}")
  except socket.gaierror:
    print(f"{d:30s} -> Could not resolve (invalid/unreachable)")






<img width="981" height="673" alt="Screenshot 2026-07-14 104133" src="https://github.com/user-attachments/assets/5b55908f-fd65-414c-9c3d-ed2f4e35b7f8" />
