def caesar_encrypt(text, shift):
   result = ""
   for ch in text:
   if ch.isalpha():
     base = ord('A') if ch.isupper() else ord('a')
     result += chr((ord(ch) - base + shift) % 26 + base)
    else:
     result += ch
    return result
def caesar_decrypt(text, shift):
  return caesar_encrypt(text, -shift)
message = "AttackAtDawn"
shift = 3
enc = caesar_encrypt(message, shift)
dec = caesar_decrypt(enc, shift)
print("Original :", message)
print("Encrypted:", enc)
print("Decrypted:", dec)





<img width="773" height="701" alt="Screenshot 2026-07-11 133237" src="https://github.com/user-attachments/assets/6de5c145-38b9-4019-8a06-f68be472c854" />
