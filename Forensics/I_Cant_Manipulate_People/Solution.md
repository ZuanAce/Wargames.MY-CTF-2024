# I Cant Manipulate People

## Challenge Description
> ![image](https://github.com/user-attachments/assets/35d3384c-39a0-47d3-8f56-5dd339e0d4c2)


## Approach
When I opened the .pcap file in Wireshark, I noticed a lot of ICMP echo requests (ping packets). 

![image](https://github.com/user-attachments/assets/ade77ddf-9907-49be-991d-c4cf9b701f7c)

Looking at the data section in these packets, I saw something interesting—it started with WGMY{.... This meant the flag was hidden in the data payloads of the ICMP packets.

![image](https://github.com/user-attachments/assets/b9b2a948-5dc7-49b9-ac48-d14883132908)

To get the flag, I needed to:
- Extract the data payloads from ICMP echo requests.
- Join the payloads into one long string.
- Convert the hexadecimal string into readable text (ASCII).

I used this command to do it:
`tshark -r traffic.pcap -Y "icmp.type == 8" -T fields -e data | tr -d '\n' | xxd -r -p`

> [!NOTE]  
> `tshark`: Filters the ICMP packets and gets the data field.
>
> `tr -d '\n'`: Joins all the hex values into one line.
> 
> `xxd -r -p`: Converts the hex string into readable text.

And finally, this gave me the flag hidden in the packets.

![image](https://github.com/user-attachments/assets/d52bb700-05d1-4552-8e46-3478174269f6)

## Flag: 
WGMY{1e3b71d57e466ab71b43c2641a4b34f4}



   




