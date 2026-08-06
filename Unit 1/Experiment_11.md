from cryptography.fernet import Fernet
# Install once: pip install cryptography
# Step 1: Generate a shared secret key (both sender & receiver need this)
key = Fernet.generate_key()
cipher = Fernet(key)
# Step 2: Sender encrypts the message before sending over the network
message = b"Transfer $500 to account 12345"
encrypted = cipher.encrypt(message)
print("Encrypted (what an eavesdropper sees):", encrypted)
# Step 3: Receiver decrypts it using the same key
decrypted = cipher.decrypt(encrypted)
print("Decrypted (what the receiver reads):", decrypted.decode())



<img width="1582" height="732" alt="Screenshot 2026-07-21 140838" src="https://github.com/user-attachments/assets/9932a42a-ad4a-4604-8c06-ebd9a6f7b97f" />
