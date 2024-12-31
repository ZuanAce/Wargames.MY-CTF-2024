# Christmas Gift

## Challenge Description
> ![image](https://github.com/user-attachments/assets/4bb99261-0f31-4bbd-97ea-073e1ca00cf7)


## Approach
Running exiftool revealed the GIF had 1402 frames. With so many frames, it was clear the flag was hidden in one of them.

![image](https://github.com/user-attachments/assets/4fc2aedb-2f1a-400e-98d2-0f9a15749e78)

When faced with the task of extracting frames from a GIF, I explored a few potential approaches via ChatGPT:
- Using Python with Pillow (PIL)
- Using FFmpeg
- Online tools like ezgif
After some thought, I opted for the Python method—it felt both efficient and satisfying for the task.

Here's the script I used:
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
> [!NOTE]  
> Install the Pillow library: `pip install pillow`
> 
> Save the script in the same directory as gift.gif.

Then, run it, and it will save all the frames in an output_frames folder.

![image](https://github.com/user-attachments/assets/694951bd-f30b-4c77-9e6d-304d8a547b80)

After extracting the frames, I carefully checked them one by one. Finally, at frame 1042, there it was, the hidden flag!

![image](https://github.com/user-attachments/assets/b2318fb4-d032-4c67-b32b-788b9bbc4272)

## Flag: 
WGMY{1eaa6da7b7f5df7c0381c8f23af4d3}



   
