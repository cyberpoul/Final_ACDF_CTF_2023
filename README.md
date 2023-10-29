<h1> Final Africa Cyber Defense Forums Cyberlympics CTF 2023 </h1>

Team: RedTeam-TG
Id: @Assa

I'll give the solution to some of the challenges that I solved

#### Web
- Grandline
- Image Lookup
- DPO Agba
- Konoha
  
#### Cryptography
- What's going on 

#### Forensics
- Detective Conan 1 

#### Steganography
- Toka 


### Web

#### Grandline:

This is an injection of graphql code, we even have a console which displays the results of the commands

First of all, we are going to do the enumeration in order to see the functions and methods that are accessible. the following payload does the trick:
![image](https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/1.png)

we can notice that we have a rather interesting flags function with the id, author, content and flag parameters.
Let's try to query the id 1 of the flags function. here I specified the content and flag parameters.
![image](https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/2.png)


We notice that we have a return with the following content: "Why don't you dig harder"; the rest becomes logical we must identify the id which contains our flag. I first tried id 2 then id 3 which turned out to be the right one.
![image](https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/3.png)

Flag: acdfCTF{L3t_try_s0m3_Graph_0ut}
