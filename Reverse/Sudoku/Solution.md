# Sudoku

## Challenge Description
> ![image](https://github.com/user-attachments/assets/60f5d27d-4712-46be-b075-ce3f1688f924)


## Approach
Unzip sudoku.zip reveals to file namely `out.enc` and `sudoku`.

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


However, changing the extension to `.jpg` didn’t help at all 🤡🤡🤡🤡🤡. When I tried opening the file, I encountered the error:
`error interpreting jpeg image file (bogus huffman table definition)`.

![image](https://github.com/user-attachments/assets/60c61d75-563c-40b7-88dd-b878701b03b9)

> [!NOTE]  
> The error message error interpreting JPEG image file (bogus Huffman table definition) indicates that there might be a corruption or unexpected data in the JPEG file.
> This is often caused by deliberate manipulation (e.g., in CTF challenges) or a problem with the file itself.

I ran exiftool to dig deeper, and noticed a strange warning: `[minor] Skipped unknown 4 bytes after JPEG APP0 segment`.

![image](https://github.com/user-attachments/assets/2119c425-f635-43a9-909b-69a738f62dfc)

This suggested there was extra, possibly unwanted data in the JPEG file. To understand the warning better, I seeked help from ChatGPT.

> [!NOTE] 
> What the Warning Means:
> - JPEG APP0 Segment: This is part of the JPEG header and is typically used to store metadata, such as JFIF (JPEG File Interchange Format) or Exif information.
> - Unknown Bytes: The extra 4 bytes found in this segment are not standard or expected according to the JPEG specification.

I then used `strings` to extract readable text from the file and, to my surprise, found an endless stream of "meows." It dawned on me: the challenge was named "Unwanted Meow," so maybe removing these "meows" would help restore the file.

![image](https://github.com/user-attachments/assets/01521494-daaf-40fc-9d0c-cc3b2c622367)

Next, I used `sed` to remove all instances of "meow" from the file: `sed 's/meow//g' flag.shredded > cleaned_flag.jpg`

> [!NOTE] 
> - s/meow//g: Replaces all occurrences of "meow" with nothing (removes them).
> - cleaned_flag.jpg: The cleaned file without "meow".

After running this command, the image was now visible, but still distorted.

![image](https://github.com/user-attachments/assets/fd2079c9-d2ed-41dc-94ea-277b954e9c99)

A second round of cleaning was needed. I ran the same sed command again on the already cleaned file: `sed 's/meow//g' cleaned_flag.jpg > very_cleaned_flag.jpg`

This time, the image finally displayed correctly, and there it was, the long-awaited flag. 🎉

![image](https://github.com/user-attachments/assets/4979dec0-881f-43b4-beb5-53203dac1ebe)

![image](https://github.com/user-attachments/assets/ce72bde5-20cb-4b99-9e31-f2cf89e23d1f)

recover key:
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
decrypt.py:
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
## Flag: 
WGMY{4a4be40c96ac6314e91d93f38043a634}



   
