# CC: Steganography

A crash course on the topic of steganography

[CC: Steganography](https://tryhackme.com/room/ccstego)

## Topic's

- Steganography

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Steghide

Steghide is one of the most famous steganography tools, and for good reason. It's a classic method, hiding a message inside an image, and steghide does it effectively and efficiently. A downside of steghide is that it only works on jpgs; however, that means that if you believe there is a hidden message inside a jpg, then steghide is a probable option.

One of the greatest benefits of stegohide, is that it can encrypt data with a passphrase. Meaning that if they don't have the password then they can't extract any data.

steghide can be installed with the command `sudo apt install steghide`

```
steghide version 0.5.1

the first argument must be one of the following:
 embed, --embed          embed data
 extract, --extract      extract data
 info, --info            display information about a cover- or stego-file
   info <filename>       display information about <filename>
 encinfo, --encinfo      display a list of supported encryption algorithms
 version, --version      display version information
 license, --license      display steghide's license
 help, --help            display this usage information

embedding options:
 -ef, --embedfile        select file to be embedded
   -ef <filename>        embed the file <filename>
 -cf, --coverfile        select cover-file
   -cf <filename>        embed into the file <filename>
 -p, --passphrase        specify passphrase
   -p <passphrase>       use <passphrase> to embed data
 -sf, --stegofile        select stego file
   -sf <filename>        write result to <filename> instead of cover-file
 -e, --encryption        select encryption parameters
   -e <a>[<m>]|<m>[<a>]  specify an encryption algorithm and/or mode
   -e none               do not encrypt data before embedding
 -z, --compress          compress data before embedding (default)
   -z <l>                 using level <l> (1 best speed...9 best compression)
 -Z, --dontcompress      do not compress data before embedding
 -K, --nochecksum        do not embed crc32 checksum of embedded data
 -N, --dontembedname     do not embed the name of the original file
 -f, --force             overwrite existing files
 -q, --quiet             suppress information messages
 -v, --verbose           display detailed information

extracting options:
 -sf, --stegofile        select stego file
   -sf <filename>        extract data from <filename>
 -p, --passphrase        specify passphrase
   -p <passphrase>       use <passphrase> to extract data
 -xf, --extractfile      select file name for extracted data
   -xf <filename>        write the extracted data to <filename>
 -f, --force             overwrite existing files
 -q, --quiet             suppress information messages
 -v, --verbose           display detailed information

options for the info command:
 -p, --passphrase        specify passphrase
   -p <passphrase>       use <passphrase> to get info about embedded data

To embed emb.txt in cvr.jpg: steghide embed -cf cvr.jpg -ef emb.txt
To extract embedded data from stg.jpg: steghide extract -sf stg.jpg
```

1. What argument allows you to embed data(such as files) into other files?

> embed, --embed embed data

`embed`

1. What flag let's you set the file to embed?

> -ef, --embedfile select file to be embedded

`-ef`

1. What flag allows you to set the "cover file"?(i.e the jpg)

> -cf, --coverfile select cover-file

`-cf`

1. How do you set the password to use for the cover file?

> -p, --passphrase specify passphrase

`-p`

1. What argument allows you to extract data from files?

> extract, --extract extract data

`extract`

1. How do you select the file that you want to extract data from?

> -sf, --stegofile select stego file

`-sf`

1. Given the passphrase "password123", what is the hidden message in the included "jpeg1" file.

```
steghide extract -sf spect/jpeg1.jpeg -p "password123"
wrote extracted data to "a.txt".

cat a.txt
pinguftw
```

`pinguftw`

## zsteg

zsteg is to png's what steghide is to jpg's. It supports various techniques to extract any and all data from png files.

Note: zsteg also supports BMP files, but it is primarily used for png's.

zsteg can be installed by using ruby with the command `gem install zsteg`

```
Usage: zsteg [options] filename.png [param_string]

    -c, --channels X                 channels (R/G/B/A) or any combination, comma separated
                                     valid values: r,g,b,a,rg,bgr,rgba,r3g2b3,...
    -l, --limit N                    limit bytes checked, 0 = no limit (default: 256)
    -b, --bits N                     number of bits, single int value or '1,3,5' or range '1-8'
                                     advanced: specify individual bits like '00001110' or '0x88'
        --lsb                        least significant BIT comes first
        --msb                        most significant BIT comes first
    -P, --prime                      analyze/extract only prime bytes/pixels
        --invert                     invert bits (XOR 0xff)
    -a, --all                        try all known methods
    -o, --order X                    pixel iteration order (default: 'auto')
                                     valid values: ALL,xy,yx,XY,YX,xY,Xy,bY,...
    -E, --extract NAME               extract specified payload, NAME is like '1b,rgb,lsb'

        --[no-]file                  use 'file' command to detect data type (default: YES)
        --no-strings                 disable ASCII strings finding (default: enabled)
    -s, --strings X                  ASCII strings find mode: first, all, longest, none
                                     (default: first)
    -n, --min-str-len X              minimum string length (default: 8)
        --shift N                    prepend N zero bits

    -v, --verbose                    Run verbosely (can be used multiple times)
    -q, --quiet                      Silent any warnings (can be used multiple times)
    -C, --[no-]color                 Force (or disable) color output (default: auto)

PARAMS SHORTCUT
        zsteg fname.png 2b,b,lsb,xy  ==>  --bits 2 --channel b --lsb --order xy
```

1. How do you specify that the least significant bit comes first

> --lsb least significant BIT comes first

`--lsb`

1. What about the most significant bit?

> --msb most significant BIT comes first

`--msb`

1. How do you specify verbose mode?

> -v, --verbose Run verbosely (can be used multiple times)

`-v`

1. How do you extract the data from a specific payload?

> -E, --extract NAME extract specified payload, NAME is like '1b,rgb,lsb'

`-E`

1. In the included file "png1" what is the hidden message?

> -s, --strings X ASCII strings find mode: first, all, longest, none (default: first)

```
steg spect/png1.png -s all
imagedata           .. file: DOS 2.0 backup id file, sequence 48
b1,r,lsb,xy         .. file: dBase III DBT, version number 0, next free block index 3234843654
b1,bgr,lsb,xy       .. text: "nootnoot$"
b2,r,lsb,xy         .. file: MacBinary, Mon Feb  6 07:28:16 2040 INVALID date, modified Mon Feb  6 07:28:16 2040 "PPPUP"
b2,b,msb,xy         .. text: "]UUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUU"
b4,r,lsb,xy         .. text: "DETUDUDUDUUUDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDDD"
b4,r,msb,xy         .. text: ["\"" repeated 243 times]
b4,g,lsb,xy         .. text: "\"\"#33223#2#2#33333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333"
b4,b,lsb,xy         .. text: "\"23\"333\"333#\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\""
b4,b,msb,xy         .. text: ["D" repeated 243 times]
b4,rgb,lsb,xy       .. text: "\"B$\"B5\"R43S%2C43S5#C4#S%2B43S5#R53S%#B$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C$2C"
b4,bgr,lsb,xy       .. text: "$\"B$2B%2S4#R53C43S%3C$#R52C43S%2S5#S%\"B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#B4#"
```

`nootnoot$`

1. What about the payload used to encrypt it.

`b1,bgr,lsb,xy`

## Exiftool

Exiftool is a tool that allows you to view and edit image metadata. While this in itself is not a stego tool, I would be remiss not to include at least a footnote on it as one of the most popular forms of image stego is to hide messages in the metadata.

Exiftool can be installed with `sudo apt install exiftool`

1. In the included jpeg3 file, what is the document name

```
exiftool spect/jpeg3.jpeg
ExifTool Version Number         : 12.05
File Name                       : jpeg3.jpeg
Directory                       : spect
File Size                       : 8.3 kB
File Modification Date/Time     : 2020:01:06 22:09:44+01:00
File Access Date/Time           : 2020:09:27 15:00:52+02:00
File Inode Change Date/Time     : 2020:09:27 15:00:44+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Big-endian (Motorola, MM)
Document Name                   : Hello :)
X Resolution                    : 1
Y Resolution                    : 1
Resolution Unit                 : None
Y Cb Cr Positioning             : Centered
Image Width                     : 213
Image Height                    : 160
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 213x160
Megapixels                      : 0.034
```

## Stegoveritas

Personally this is one of my favorite image stego tools. It supports just about every image file, and is able to extract all types of data from it. It is an incredibly useful tool if you don't know exactly what you're looking for, as it has a myriad of built in tests to extract any and all data.

Note: Stegoveritas has other features as well such as color correcting images

Stegoveritas can be installed by running these two commands:

- `pip3 install stegoveritas`
- `stegoveritas_install_deps`

```
usage: stegoveritas [-h] [-out dir] [-debug] [-password PASSWORD] [-wordlist WORDLIST] [-meta] [-imageTransform] [-bruteLSB] [-colorMap [N [N ...]]] [-colorMapRange Start End] [-extractLSB]
                    [-red index [index ...]] [-green index [index ...]] [-blue index [index ...]] [-alpha index [index ...]] [-extract_frames] [-trailing] [-steghide] [-exif] [-xmp]
                    [-carve]
                    file

Yet another Stego tool

positional arguments:
  file                  The file to analyze

optional arguments:
  -h, --help            show this help message and exit
  -out dir              Directory to place output in. Defaults to ./results
  -debug                Enable debugging logging.
  -password PASSWORD    When applicable, attempt to use this password to extract data.
  -wordlist WORDLIST    When applicable, attempt to brute force with this wordlist.

image options:
  -meta                 Check file for metadata information
  -imageTransform       Perform various image transformations on the input image and save them to the output directory
  -bruteLSB             Attempt to brute force any LSB related stegonography.
  -colorMap [N [N ...]]
                        Analyze a color map. Optional arguments are colormap indexes to save while searching
  -colorMapRange Start End
                        Analyze a color map. Same as colorMap but implies a range of colorMap values to keep
  -extractLSB           Extract a specific LSB RGB from the image. Use with -red, -green, -blue, and -alpha
  -red index [index ...]
  -green index [index ...]
  -blue index [index ...]
  -alpha index [index ...]
  -extract_frames       Split up an animated gif into individual frames.
  -trailing             Check for trailing data on the given file
  -steghide             Check for StegHide hidden info.

multi options:
  -exif                 Check this file for exif information.
  -xmp                  Check this file for XMP information.
  -carve                Attempt to carve/extract things from this file.

Have a good example? Wish it did something more? Submit a ticket: https://github.com/bannsec/stegoVeritas
```

1. How do you check the file for metadata?

> -meta Check file for metadata information

`-meta`

1. How do you check for steghide hidden information

> -steghide Check for StegHide hidden info.

`-steghide`

1. What flag allows you to extract LSB data from the image?

> -extractLSB Extract a specific LSB RGB from the image. Use with -red, -green, -blue, and -alpha

`-extractLSB`

1. In the included image jpeg2 what is the hidden message?

```
cat results/steghide_5e3b4b8fc262e154cb349f94e58c3026.bin
kekekekek
```

`kekekekek`

## Spectrograms

Spectrogram stegonography is the art of hiding hidden an image inside in an audio file's spectogram. Therefore when ever dealing with audio stego it is always worth analyzing the spectrogram of the audio. To do this task we will be using Sonic Visualizer.

Note: This introduction will be done using the included wav1 file.

When you open Sonic Visualizer you should see this screen:

![](https://i.imgur.com/CKHI4le.png)

From there click File->Open and then select the included wav1 file and you should see a screen similar to this:

![](https://i.imgur.com/8TexIob.png)

From there click Layer->Add Spectrogram and you should see this:

![](https://i.imgur.com/wHSr0NM.png)

And that's it!

1. What is the hidden text in the included wav2 file?

`Google`

## The Final Exam

Good luck and have fun!

1. What is key 1?

```
stegoveritas -exif exam1.jpeg
Running Module: SVImage
+------------------+------+
|   Image Format   | Mode |
+------------------+------+
| JPEG (ISO 10918) | RGB  |
+------------------+------+
Running Module: MultiHandler

Exif
====
+---------------------+-----------------------------------------------------------------+
| key                 | value                                                           |
+---------------------+-----------------------------------------------------------------+
| SourceFile          | /home/kali/Downloads/ctf/tryhackme/CC: Steganography/exam1.jpeg |
| ExifToolVersion     | 12.06                                                           |
| FileName            | exam1.jpeg                                                      |
| Directory           | /home/kali/Downloads/ctf/tryhackme/CC: Steganography            |
| FileSize            | 8.6 kB                                                          |
| FileModifyDate      | 2020:09:27 15:42:16+02:00                                       |
| FileAccessDate      | 2020:09:27 15:42:23+02:00                                       |
| FileInodeChangeDate | 2020:09:27 15:42:16+02:00                                       |
| FilePermissions     | rw-r--r--                                                       |
| FileType            | JPEG                                                            |
| FileTypeExtension   | jpg                                                             |
| MIMEType            | image/jpeg                                                      |
| JFIFVersion         | 1.01                                                            |
| ExifByteOrder       | Big-endian (Motorola, MM)                                       |
| DocumentName        | password=admin                                                  |
| XResolution         | 1                                                               |
| YResolution         | 1                                                               |
| ResolutionUnit      | None                                                            |
| YCbCrPositioning    | Centered                                                        |
| ImageWidth          | 213                                                             |
| ImageHeight         | 160                                                             |
| EncodingProcess     | Baseline DCT, Huffman coding                                    |
| BitsPerSample       | 8                                                               |
| ColorComponents     | 3                                                               |
| YCbCrSubSampling    | YCbCr4:2:0 (2 2)                                                |
| ImageSize           | 213x160                                                         |
| Megapixels          | 0.034                                                           |
+---------------------+-----------------------------------------------------------------+
```

`password=admin`

```
steghide extract -sf exam1.jpeg -p "admin"
the file "a.txt" does already exist. overwrite ? (y/n) y
wrote extracted data to "a.txt".

kali@kali:~/Downloads/ctf/tryhackme/CC: Steganography$ cat a.txt
the key is: superkeykey
```

`superkeykey`

2. What is key 2?

`https://imgur.com/KTrtNl5`

```
zsteg exam2.png -s all
imagedata           .. text: ")))xxxLMO"
b1,bgr,lsb,xy       .. text: "\rKey: fatality"
b2,rgb,lsb,xy       .. file: SoftQuad DESC or font file binary
b2,rgb,msb,xy       .. file: VISX image file
b2,bgr,lsb,xy       .. file: SoftQuad DESC or font file binary
b2,bgr,msb,xy       .. file: VISX image file
```

3. What is key 3?

`http://key=killshot`

`killshot`
