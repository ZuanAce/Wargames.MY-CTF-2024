# Sudoku

## Challenge Description
> ![image](https://github.com/user-attachments/assets/60f5d27d-4712-46be-b075-ce3f1688f924)


## Approach
Unzip sudoku.zip reveals to file namely `out.enc` and `sudoku`.

`out.enc`:
`z v7o1 an7570 9d.tl3 7.4b 7n2pws .qodx v7oc ye68u m.7r, t728{09er1bzbs9sx5sosu7719besr39zscbx}`

Running the `file` command on sudoku does not give much information. I tried opening the file using [Decompiler Explorer](https://dogbolt.org/) and it shows `error: file to large`. 

Then, I used the `strings` command, and i found something at the end: `pydata`, seems like the program is definitely compiled using python

![image](https://github.com/user-attachments/assets/7d8b1625-2696-421a-9fe9-39e6e54e5970)

Exploring and researching leads me to [PyInstaller Extractor WEB](https://pyinstxtractor-web.netlify.app/).

```
[+] Please stand by...
[+] Processing sudoku
[+] Pyinstaller version: 2.1+
[+] Python library file: libpython3.11.so.1.0
[+] Python version: 3.11
[+] Length of package: 7574445 bytes
[+] Found 31 files in CArchive
[+] Beginning extraction...please standby
[+] Possible entry point: pyiboot01_bootstrap.pyc
[+] Possible entry point: pyi_rth_inspect.pyc
[+] Possible entry point: sudoku.pyc
[+] Found 99 files in PYZArchive
[+] Successfully extracted pyinstaller archive: sudoku

You can now use a python decompiler on the pyc files within the extracted directory
[+] Extraction completed successfully, downloading zip
```

At, the `sudoku_extracted folder`, there are many files extracted. However, we only need to decompile `sudoku.pyc`.

![image](https://github.com/user-attachments/assets/8e90103f-226c-4769-88a7-590224422dc9)

Then, i used [Pylingual](https://pylingual.io/) to decompile `sudoku.pyc`:

![image](https://github.com/user-attachments/assets/1577a2a7-424b-441b-a04a-6112c758485f)

> [!NOTE]
> This algorithm encrypts a message (the `plaintext`) using a simple substitution cipher:
> 1. **Alphabet**: A predefined set of characters (`a-z`, numbers, and `.`) that can be used for encryption.
> 2. **Key Generation**: A random rearrangement of the `alphabet` is created, called the `key`.
> 3. **Encryption**:
>     - Each character in the `plaintext` is replaced by its corresponding character from the `key` (based on the original `alphabet`).
>     - Characters not in the `alphabet` (like spaces or special symbols) remain unchanged.
> 
> The result is an encrypted version of the message (`enc`), where the original text is hidden by the substitution.

Since I have both the plaintext and its encrypted counterpart, you can derive the key by mapping the corresponding characters from the plaintext to the encrypted text. Here’s a script to extract the key:
```python
plaintext = '0 t.e1 qu.c.2 brown3 .ox4 .umps5 over6 t.e7 lazy8 do.9'
encrypted = 'z v7o1 an7570 9d.tl3 7.4b 7n2pws .qodx v7oc ye68u m.7r'
alphabet = 'abcdelmnopqrstuvwxyz1234567890.'

def find_key(plaintext, encrypted, alphabet):
    # Create a mapping from plaintext to encrypted characters
    key_map = {}
    for p_char, e_char in zip(plaintext, encrypted):
        if p_char.lower() in alphabet:
            key_map[p_char.lower()] = e_char.lower()
    
    # Generate the key by ordering according to the original alphabet
    key = ''.join(key_map.get(c, '?') for c in alphabet)
    print(key_map)
    return key

key = find_key(plaintext, encrypted, alphabet)
print("Recovered Key:", key)
```
> [!NOTE]
> - Only part of the enc was chosen because REDACTED would be useless in this case
> - The script iterates over plaintext and encrypted simultaneously using zip.
> - For each pair of characters, if the plaintext character is in the alphabet, it maps the plaintext character to the encrypted character.
> - The script constructs the key by ordering the characters in the alphabet sequence using the key_map.
> - Any missing mappings are replaced with ? in the key.

Running the script gives us the recivered key: `e95moy2l.padwvnqt486103bsxcurz7`

![image](https://github.com/user-attachments/assets/c9971d9b-b0ef-46ec-a589-e3a9bd208b7e)

After recovering the key, it's time to decrypt the encrypted message. Here's the script for the decryption:
```python
alphabet = 'abcdelmnopqrstuvwxyz1234567890.'
key = 'e95moy2l.padwvnqt486103bsxcurz7'
encrypted = 'z v7o1 an7570 9d.tl3 7.4b 7n2pws .qodx v7oc ye68u m.7r, t728{09er1bzbs9sx5sosu7719besr39zscbx}'

def decrypt(encrypted, key, alphabet):
    # Create a reverse mapping from key to alphabet
    reverse_key_map = dict(zip(key, alphabet))
    
    # Decrypt the text
    decrypted_text = ''.join(reverse_key_map.get(c.lower(), c) for c in encrypted)
    return decrypted_text

# Decrypt the text
decrypted_text = decrypt(encrypted, key, alphabet)

print("Decrypted Text:", decrypted_text)
```

> [!NOTE]
> Reverse Key Mapping:
> - A reverse_key_map dictionary is created where each character in the key maps to the corresponding character in the alphabet.
>
> Decryption Process:
> - Iterate through each character in the encrypted text.
> - If the character exists in the key, replace it with the corresponding character from the alphabet.
> - If the character does not exist in the key (e.g., spaces, punctuation), keep it unchanged.
>
> Decrypted Output:
> - The decrypted text should match the original plaintext.

Running the script, making the flag looks visible: `w.my{2ba914045b56c5e58..1b4a593b05746}`

![image](https://github.com/user-attachments/assets/11b6e785-7967-4673-a331-6afcd5a86707)

Looks like we have to fill in the dots.
From `alphabet = 'abcdelmnopqrstuvwxyz1234567890.'`, we can seee that the missing alphabets `f,g,h,i,j,k`

z v7o1 an7570 9d.tl3 7.4b 7n2pws .qodx v7oc ye68u m.7r, t728{09er1bzbs9sx5sosu7719besr39zscbx}
0 t.e1 qu.c.2 brown3 .ox4 .umps5 over6 t.e7 lazy8 do.9, w.my{2ba914045b56c5e58..1b4a593b05746}
fghijk

## Flag: 
WGMY{4a4be40c96ac6314e91d93f38043a634}



   
