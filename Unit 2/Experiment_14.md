from google.colab import files

uploaded = files.upload()

import hashlib

# Example malware hash database
malware_hashes = {
    "44d88612fea8a8f36de82e1278abb02f4fcf5e6d7d8b4f0c1f5f5c6d6c3f8f3a",
    "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
}

def calculate_sha256(file_path):
    sha256 = hashlib.sha256()

    with open(file_path, "rb") as f:
        while True:
            data = f.read(4096)
            if not data:
                break
            sha256.update(data)

    return sha256.hexdigest()

# Get uploaded file name
file_path = list(uploaded.keys())[0]

file_hash = calculate_sha256(file_path)

print("File Name:", file_path)
print("SHA-256 Hash:", file_hash)

if file_hash in malware_hashes:
    print("⚠️ Malware Detected!")
else:
    print("✅ File is Safe (Hash not found in malware database).")



<img width="1515" height="698" alt="image" src="https://github.com/user-attachments/assets/84bec56b-4749-468b-8eb3-3c513c692fc0" />
