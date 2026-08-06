import hashlib
def hash_file(path):
  with open(path, "rb") as f:
    return hashlib.sha256(f.read()).hexdigest()
# Create sample files: one original, one copy, one tampered
with open("original.txt", "w") as f: f.write("System32 backup data")
with open("copy.txt", "w") as f: f.write("System32 backup data")
with open("tampered.txt", "w") as f: f.write("System32 backup data - modified")
files = ["original.txt", "copy.txt", "tampered.txt"]
hashes = {f: hash_file(f) for f in files}
baseline = hashes["original.txt"]
print("Baseline (original.txt):", baseline)
for f in files[1:]:
  status = "IDENTICAL" if hashes[f] == baseline else "MODIFIED/TAMPERED"
  print(f"{f}: {status}")


<img width="991" height="640" alt="Screenshot 2026-07-21 141444" src="https://github.com/user-attachments/assets/1f56ed91-b961-4e63-b8d6-9c2ec132de99" />
