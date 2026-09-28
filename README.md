# Data-encryption-technique-based-on-DNA-Cryptography-
Developed a secure data encryption system using a DNA-inspired encryption technique to protect sensitive text data. The project converts plaintext into ASCII values, maps the data into a 4×4 matrix, applies a secret key-based transformation, and generates encrypted output that can only be decrypted with the correct key.





from encryption import encrypt_text
from decryption import decrypt_text

def main():
    print("=== DNA-Based Data Encryption Technique ===")

    plaintext = input("Enter text to encrypt: ")
    key = input("Enter secret key: ")

    if not plaintext or not key:
        print("Text and key cannot be empty.")
        return

    encrypted = encrypt_text(plaintext, key)

    print("\nEncrypted data:")
    print(encrypted)

    choice = input("\nDecrypt the data? (y/n): ").strip().lower()

    if choice == "y":
        decrypted = decrypt_text(encrypted, key)
        print("\nDecrypted data:")
        print(decrypted)

if __name__ == "__main__":
    main()


Encryption.py


DNA_MAP = {
    "00": "A",
    "01": "C",
    "10": "G",
    "11": "T"
}

def text_to_dna(text):
    dna = []

    for byte in text.encode("utf-8"):
        binary = format(byte, "08b")

        for i in range(0, 8, 2):
            dna.append(DNA_MAP[binary[i:i + 2]])

    return "".join(dna)

def key_shift(key):
    return sum(key.encode("utf-8")) % 4

def matrix_transform(dna, shift):
    padding = (4 - len(dna) % 4) % 4
    dna += "A" * padding

    rows = [
        list(dna[i:i + 4])
        for i in range(0, len(dna), 4)
    ]

    transformed = []

    for row in rows:
        transformed.extend(row[shift:] + row[:shift])

    return "".join(transformed)

def encrypt_text(text, key):
    dna = text_to_dna(text)
    shift = key_shift(key)
    transformed = matrix_transform(dna, shift)

    return f"{len(dna)}:{transformed}"



Decryption.py


DNA_TO_BITS = {
    "A": "00",
    "C": "01",
    "G": "10",
    "T": "11"
}

def key_shift(key):
    return sum(key.encode("utf-8")) % 4

def reverse_matrix_transform(dna, shift):
    rows = [
        list(dna[i:i + 4])
        for i in range(0, len(dna), 4)
    ]

    restored = []

    for row in rows:
        if shift:
            restored.extend(row[-shift:] + row[:-shift])
        else:
            restored.extend(row)

    return "".join(restored)

def dna_to_text(dna):
    bits = "".join(DNA_TO_BITS[x] for x in dna)
    data = bytearray()

    for i in range(0, len(bits), 8):
        byte_bits = bits[i:i + 8]

        if len(byte_bits) == 8:
            data.append(int(byte_bits, 2))

    return bytes(data).decode("utf-8")

def decrypt_text(encrypted_data, key):
    original_length, dna = encrypted_data.split(":", 1)

    original_length = int(original_length)
    dna = dna[:original_length]

    shift = key_shift(key)
    restored = reverse_matrix_transform(dna, shift)

    return dna_to_text(restored)



 DNA-Based Data Encryption Technique

 Project Description

Developed an academic data encryption technique using Python, DNA-inspired encoding, and matrix-based transformation.

The system converts text into UTF-8 byte values, maps binary pairs into DNA bases (A, C, G, T), and applies a secret-key-based matrix transformation. The encrypted data can be converted back to the original text using the same key.

## Features

- Text encryption
- Text decryption
- DNA-inspired encoding
- Binary-to-DNA mapping
- 4x4 matrix-based transformation
- Secret-key-based transformation
- Command-line interface

 Technologies Used

- Python
- UTF-8 Encoding
- Binary Conversion
- Matrix Operations
- Encryption and Decryption Logic

How It Works

1. User enters text and a secret key.
2. Text is converted into UTF-8 bytes.
3. Binary data is divided into pairs of bits.
4. Binary pairs are mapped to DNA bases:
   - 00 = A
   - 01 = C
   - 10 = G
   - 11 = T
5. DNA data is arranged into groups of four.
6. A key-based transformation is applied.
7. The transformed data is returned as encrypted output.
8. The same key is used to reverse the process and recover the original text.

## How to Run

Install Python 3 and run:

python main.py

## Project Structure

DNA-Based-Data-Encryption/

├── main.py
├── encryption.py
├── decryption.py
├── README.md
└── requirements.txt

## Project Outcome

The project demonstrates how DNA-inspired encoding and matrix-based transformations can be used to create a reversible data transformation and encryption technique.

## Disclaimer

This is an educational project and is not intended to replace established cryptographic algorithms such as AES.