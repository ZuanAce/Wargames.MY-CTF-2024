# Credentials

## Challenge Description
> ![image](https://github.com/user-attachments/assets/0761bd52-c8aa-433e-a8cd-a1e239774f24)


## Approach
We were given a Leaked.rar file, which, when unzipped, contained two text files: user.txt and passwd.txt.

![image](https://github.com/user-attachments/assets/dfa665ce-607e-4a90-ad4e-541d2fb716e2)

Opening the user.txt, I quickly searched for osman, as the challenge hinted. It was located at line 337.

![image](https://github.com/user-attachments/assets/7c3328a3-bf40-4bfc-a93d-a52f70d6bc03)

I then turned to passwd.txt to find the corresponding password for osman. Sure enough, at line 337, I found the encoded password:

![image](https://github.com/user-attachments/assets/289a3c05-eb4b-4a35-aae0-ec603ebecb24)

The password was in the form of ZJPB{e6g180g9f302g8d8gddg1i2174d0e212}, which clearly needed decoding. I used [Cipher Identifier](https://www.dcode.fr/cipher-identifier) to analyze the encoding, and the tool suggested several possibilities:
- Periodic Table Cipher
- Hexadecimal Data
- ROT Cipher
- Caesar Cipher
- Mono-alphabetic Substitution

After reviewing these options, the Caesar Cipher stood out as the most likely choice.

I applied the [Caesar Cipher](https://www.dcode.fr/caesar-cipher) to the encoded text and, after decrypting it, I was able to retrieve the flag!

![image](https://github.com/user-attachments/assets/322b2a2a-ebc3-4030-bd4d-ad26287fd78a)

## Flag: 
wgmy{b6d180d9c302d8a8daad1f2174a0b212}


   



