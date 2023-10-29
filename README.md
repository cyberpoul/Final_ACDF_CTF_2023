<h1> Final Africa Cyber Defense Forums Cyberlympics CTF 2023 </h1>

Team: RedTeam-TG

Id: Assa

I'll give the solutions to some of the challenges that I solved but you can check the full writeups of the team here:
<br>
https://hackmd.io/@RedTeamTG/acdfctf-final-2023

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
</details>

<br>
<details><summary>Web :</summary>
  
- Grandline:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/7.png" alt="img"></p>

This is an injection of graphql code, we even have a console which displays the results of the commands

First of all, we are going to do the enumeration in order to see the functions and methods that are accessible. the following payload does the trick:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/1.png" alt="img"></p>

we can notice that we have an interesting function called "flags" with the id, author, content and flag parameters.
Let's try to query the id 1 of the flags function. here I specified the content and flag parameters.

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/2.png" alt="img"></p>


We notice that we have a return with the following content: "Why don't you dig harder"; the rest becomes logical we must identify the id which contains our flag. I first tried id 2 then id 3 which turned out to be the right one.

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/3.png" alt="img"></p>

Flag: acdfCTF{L3t_try_s0m3_Graph_0ut}


- Image Lookup:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/6.png" alt="img"></p>

This one is an easy lfi web challenge you just had to call the flag file with the following payload: "file:///flag.txt" , paying attention to encoding the characters /
here is the query that I used to get the flag

```http://16.170.230.246/index.php?url=file%3a%2f%2f%2fflag.txt```

Unfortunately the flag is no longer accessible on the server


- Konoha:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/4.png" alt="img"></p>

we have a source.php file. let's try to analyze it:
```python
<?php
$secretFilePath = '/app/Sup3rs3cr3tFlag.txt';

$secretKey = 'Kismet-Abzee-Berrywuxxxxx';

$requestedFile = isset($_GET['file']) ? $_GET['file'] : '';

$providedKey = isset($_GET['key']) ? $_GET['key'] : '';

$decodedFile = urldecode($requestedFile);

if ($providedKey !== $secretKey) {
    header("HTTP/1.0 403 Forbidden");
    echo "Access denied!";
    exit;
}

if ($decodedFile === 'Sup3rs3cr3tFlag.txt') {
    $secretContent = file_get_contents($secretFilePath);
    echo $secretContent;
} else {
    header("HTTP/1.0 403 Forbidden");
    echo "Access denied!";
}
?>
```
after reading we see that the source.php script gives us the possibility of making GET type requests with the "file" and "key" parameters. key is then the secret code allowing access to the file and file must contain the name of the file. 
```
if ($decodedFile === 'Sup3rs3cr3tFlag.txt') {
     $secretContent = file_get_contents($secretFilePath);
     echo $secretContent;
}
```
the previous lines check that the file name corresponds to Sup3rs3cr3tFlag.txt and displays this secret message to us 
```echo $secretContent;```
what could be simpler we have the name of the file and the key to access it.
be careful in the key value we have hidden characters (5 characters x) 'Kismet-Abzee-Berrywuxxxxx'.
It is therefore necessary to brute force the missing x characters. the hint gave us part of the missing characters, we just had to brute force the rest to obtain the flag.
Ps: apparently the 5 missing characters x could be found in the source code of the application personally I didn't solve it like that.
here is the final request to have the flag:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/5.png" alt="img"></p>

The flag has unfortunately been removed from the server. I unfortunately didn't take a picture when I solved it but the request on the picture still exact.

- DPO Agba:

Coming soon i need a bit of rest lol
</details>
