# Invisible Ink

## Challenge Description
> ![image](https://github.com/user-attachments/assets/e380813e-3517-43db-8122-5eb17e7ca099)


## Approach
So, it'a another `.gif` file. I started by examining the GIF file with exiftool. Here’s what I found:

![image](https://github.com/user-attachments/assets/b019e02d-c2fd-4191-89d6-4ba43f47d322)

> [!NOTE]
> Here's what stands out:
> - File Type: GIF (89a version)
> - Dimensions: 400x400 pixels
> - Transparent Color: The transparent color index is 2, which means the third color in the color table is transparent.
> - Animation: 7 frames, with infinite looping.

I initially tried extracting the frames using the Python script I had previously used for the **Christmas Gift** chall:
```python
from PIL import Image
import os

def extract_frames(gif_path, output_folder):
    os.makedirs(output_folder, exist_ok=True)
    with Image.open(gif_path) as img:
        for frame in range(img.n_frames):
            img.seek(frame)
            frame_path = os.path.join(output_folder, f"frame_{frame}.png")
            img.save(frame_path)
            print(f"Saved {frame_path}")

extract_frames("gift.gif", "output_frames")
```
However, this time, the script only managed to extract the first few frames. The last three stubbornly refused to cooperate.

![image](https://github.com/user-attachments/assets/e27cbd82-442b-477e-ba3a-5d6ab3021a3b)

Time to switch gears. I turned to [Stegsolve](https://github.com/manisashank/stegsolve/tree/master), after some explorations and researches.


To extract the frames from `challenge.gif`, 
1. Download and run Stegsolve following the instructions from [GitHub](https://github.com/manisashank/stegsolve/tree/master).
2. Open `challenge.gif` in Stegsolve:
   -  Navigate to `File` > Open and select the GIF.
3. Use the `Frame Browser` (under Analyse) to review each frame.
4. In the Frame Browser, I noticed that Frame 5 and Frame 6 looked suspicious. There seemed to be hidden text in the center of these frames. I saved both frames for further analysis:

   | Frame 5 (frame5.bmp)                                                                      | Frame 6 (frame6.bmp)                                                                        |
   | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
   | ![image](https://github.com/user-attachments/assets/43693311-a2df-4d28-a926-c962385cddd1) |  ![image](https://github.com/user-attachments/assets/7eba9926-5f58-43ab-92d8-79560ec3bba1)  |

To reveal the hidden content,
1. I applied random color maps to the saved frames using Stegsolve. This made the text readable:

  | Frame 5 (solvedframe5.bmp)                                                                | Frame 6 (solvedframe6.bmp)                                                                  |
  | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
  | ![image](https://github.com/user-attachments/assets/1dc5a7af-c285-4a9d-abf3-1486465685df) |  ![image](https://github.com/user-attachments/assets/b21c553a-127c-49be-833b-30647718d81a)  |

2. Save these two frames.

The final step was to combine the two frames to reveal the complete flag:
1. Open solvedframe6.bmp in Stegsolve.
2. Navigate to `Analyse` > `Image Combiner` and select `solvedframe5.bmp`.
3. The combined image displayed the flag clearly in the center!

   ![image](https://github.com/user-attachments/assets/19d3e191-aad8-44ef-9fc3-df8e000c0067)

## Flag: 
wgmy{d41d8cd98f00b204e9800998ecf8427e}




   
