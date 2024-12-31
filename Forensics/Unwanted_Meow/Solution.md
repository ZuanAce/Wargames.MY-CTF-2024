# Unwanted Meow

## Challenge Description
> ![image](https://github.com/user-attachments/assets/5cabae94-0a40-4164-b663-073c8bea9689)


## Approach
Since is a strange extension, I checked the file with the `file` command and saw that it contained JPEG image data. So, I thought changing the extension will help.

![image](https://github.com/user-attachments/assets/3e2bb250-e37a-499d-958e-aa77eab28835)

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

![image](https://github.com/user-attachments/assets/c4b8ddb4-4e23-4434-87ff-02e44692c107)

## Flag: 
wgmy{4a4be40c96ac6314e91d93f38043a634}



   



