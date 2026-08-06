import hashlib

def file_hash(filename, algo="sha256"):
  h = hashlib.new(algo)
  with open(filename, "rb") as f:
    h.update(f.read())
  return h.hexdigest()

with open("sample.txt", "w") as f:
  f.write("Confidential report v1")

print("Original hash :", file_hash("sample.txt"))

with open("sample.txt", "a") as f:
  f.write(" - tampered!")

print("Modified hash :", file_hash("sample.txt"))
print("Integrity check: FAILED (hash changed) -> file was modified")



<img width="933" height="867" alt="Screenshot 2026-07-11 140343" src="https://github.com/user-attachments/assets/a04bc67c-2ba0-43c6-b946-434c6d3f41df" />
