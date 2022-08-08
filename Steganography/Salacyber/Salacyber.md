# Salacyber

## Overview 
Points: 15

Category: Steganography

## Description
Something in the blue...

## Approach

The file appears to be an image with extension jpeg. 

There's no information hidden in plain sign found from the result of using `strings`. `Binwalk` could'nt any signature of any hidden embeded file inside. So It seems that i got no luck with `strings` and `binwalk`.

Let's view the image metadata using `exiftool`. 

```
touexe@kali:~$ exiftool salacyber_f08e6d459baa29f3b98c49.jpeg          
ExifTool Version Number         : 12.42
File Name                       : salacyber_f08e6d459baa29f3b98c49.jpeg
Directory                       : .
File Size                       : 103 kB
File Modification Date/Time     : 2022:07:30 11:23:43+07:00
File Access Date/Time           : 2022:08:01 16:15:32+07:00
File Inode Change Date/Time     : 2022:07:30 11:23:44+07:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
X Resolution                    : 1
Y Resolution                    : 1
Exif Byte Order                 : Big-endian (Motorola, MM)
Resolution Unit                 : None
Artist                          : s@lacyb3r
Y Cb Cr Positioning             : Centered
Image Width                     : 2048
Image Height                    : 2048
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2048x2048
Megapixels                      : 4.2
```
Nothing to be found important here except the artist tag value `s@lacyb3r` which seems to be a hint or a password or the flag itself? No, We got no luck with that `s@lacyb3r` as the flag, yet to be either a hint or a password. 

I came across [`steghide`](http://steghide.sourceforge.net/) which is is a steganography tool that uses a passcode to hide private files within the image or audio file. BMP and JPEG picture types are supported, as well as AU and WAV audio formats. The file is encrypted by default using the Rijndael algorithm, with a key size of 128 bits. However, There's a tool to counter `steghide` itself. [`stegseek`](https://github.com/RickdeJager/stegseek) is a `steghide` cracker that can be used to extract hidden data from files.

From software instruction we got this command to find hidden data inside the image.

```
touexe@kali:~$ stegseek --seed salacyber_f08e6d459baa29f3b98c49.jpeg 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found (possible) seed: "5db71817"             
	Plain size: 76.0 Byte(s) (compressed)
	Encryption Algorithm: rijndael-128
	Encryption Mode:      cbc
```
Yup! definietely a hidden file inside the image using steghide with passphrase.

Where's the passphrase tho? From our previous discovery. `s@lacyb3r` seems to be there for a reason as either a hint or a password to somewhere. Let's try it out. 

```
touexe@kali:~$ steghide extract -sf salacyber_f08e6d459baa29f3b98c49.jpeg         
Enter passphrase: s@lacyb3r
wrote extracted data to "nothing_here_too.txt".
```
We did it! A text file was extracted. By viewing the file we can see the flag is right inside it.

## Flag

**salacyber{s@lacyb3r_ctf_ch@ll3ng3}**

## Aditional Note

we use this website https://futureboy.us/stegano/decinput.html to decrypt and extract the file which is exactly an online version of `steghide`
