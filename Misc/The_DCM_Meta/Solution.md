# The DCM Meta

## Challenge Description
> ![image](https://github.com/user-attachments/assets/fa89a60a-ca24-4f0f-915c-7c49347d4717)


## Approach
No idea what a `.dcm` file is, so I used the `file` command to identify the type of file. It returned just "data," which didn’t tell me much. 

![image](https://github.com/user-attachments/assets/94f17aab-c0a6-41fb-a0a1-bc7839d99dd8)

So, I decided to inspect the file's raw content using xxd. Running xxd revealed this gem:

![image](https://github.com/user-attachments/assets/e18716d0-43ef-4286-93f8-fdbd880d1059)

`WGMYf63acd3b78127c1d7d3e700b55665354`

The prefix WGMY was followed by a 32-character string, matching the 32 numbers in the challenge's list. From the hint, it was clear that the flag was hidden by rearranging these characters according to the order given.

Here's the python script I used:
```python
# Input string and order list
input_string = "f63acd3b78127c1d7d3e700b55665354"
order = [25, 10, 0, 3, 17, 19, 23, 27, 4, 13, 20, 8, 24, 21, 31, 15, 7, 29, 6, 1, 9, 30, 22, 5, 28, 18, 26, 11, 2, 14, 16, 12]

# Rearrange the string based on the order list
rearranged = ''.join(input_string[i] for i in order)

# Wrap the result with wgmy{}
result = f"wgmy{{{rearranged}}}"

# Print the result
print(result)
```
> [!NOTE]
> Here's how it works 
> - The order list specifies the indices to rearrange the characters in the input string.
> - A list comprehension is used to extract characters from input_string in the specified order.
> - The result is wrapped with wgmy{}.
> - The rearranged string is printed.

After running the script, it printed the flag:

![image](https://github.com/user-attachments/assets/9d337ff6-b160-4591-8c5e-232297345e2a)

## Flag: 
wgmy{51fadeb6cc77504db336850d53623177}




   
