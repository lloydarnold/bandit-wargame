# Bandit -- Stage by Stage explanation

## Level 0
All you have to do here is ssh onto the bandit server. This can be done with the command 
`$ ssh -p 2220 bandit0@bandit.labs.overthewire.org` 
and then entering the password `bandit0`

## Level 1
The password for the next level is stored in a file on the desktop named 'readme'.

This can be revealed with the `ls` command, and read with `cat readme`.

The password for bandit1 is ` boJ9jbbUNNfktd78OOpsqOltutMc3MY1 `

## Level 2
The password here is stored in a file called '-'

This can be revealed with the `ls` command, but running `cat -` confuses the system, as `-` normally denotes a flag. We can get around this by using the complete file path, './-'. This gives us the command `cat ./-`

The password for bandit2 is ` CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9 `

## Level 3
The password for the next level is stored in a file called spaces in this filename located in the home directory

This cannot be accessed by simply running `cat spaces in this file name`, as the system will register this as four separate files. Instead, we can either use the escape character, '\', in front of the spaces, or specify the file name in quotation marks. This gives us either `cat spaces\ in\ this\ filename` or ` cat "spaces in this file name" `

The password for bandit3 is ` UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK `

## Level 4
The password for the next level is stored in a hidden file in the inhere directory.

We can change into this directory using `cd inhere`, and then reveal the hidden file, which is called '.hidden'. We can then read the contents of this file with the command `cat .hidden `.

The password for bandit4 is ` pIwrPrtPN36QITSp3EQaw936yaFoFgAB `

## Level 5
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

Again, we can change to the inhere directory using `cd inhere`.
Running `ls` then reveals that there are ten files in this directory. 
As we know that the password is contained in a human readable file, we need to somehow filter the files by filetype. This can be done with the command 
``` find . -type f | xargs file```. 
This works by finding all the files in the current directory, and passing these as an argument into the file command, which then lists them by type. Doing so reveals to us that 9 of the 10 files are of type 'data', but one is of type 'ASCII text'. Therefore, the password must be in this file. 

This works given the relatively small number of files in this directory, but what if it were larger?
We could extend this command by piping the output into ` grep text`, giving us a complete command of 

``` find . -type f | xargs file | grep text```

This would return only the file marked as ASCII text, omitting all others. 

The password for bandit5 is koReBOKuIDDepwhWk7jZC0RTdopnAYKh

## Level 6
In this level, we are given three properties of the file containing the password. It is human readable, 1033 bytes and not executable.

Therefore, we need to somehow string these conditions together into a search command. We can again start with `find . -type f`, which is going to recursively find all files of in this directory and subdirectories. However, this gives an unusably long output. At this point, one might think to apply ` | xargs file | grep text`, as in the previous level.

This was tried, but still presented the issue of giving an unworkably large number of files. For that reason, I changed the command used, and introduced the `-exec` flag to my initial find statement. This allows us to also run ls with the appropriate flags (to list more information on the file, including file size and whether it is executable). 
The flags I used were -la; l is the alias of long listing, giving more information, and a is the alias for all, meaning even hidden files are included. From here, I just needed to add a filter so that only files 1033 bytes long were shown - this is done using the `grep 1033` command. By piping these together, we are left with

``` find . type -f -exec ls -la {} \; | grep 1033 ```

The password for bandit6 is DXjZPULLxYr17uwoI01bNLQbtFemEgo7

## Level 7
The password for the next level is stored somewhere on the server and has all of the following properties:

* Owned by user Bandit7
* Owned by group Bandit6
* 33 bytes in size


Immediately, we know that we wish to search the entire file system. This can be done with ` find / `.
As before, we know that we want to look for all files, so we can introduce the flag `-type f`. We can then make use of the `-user`, `-group` and `-size` flags for the find command, giving us:

``` $ find / -user bandit7 -group bandit6 -type f -size 33c ```

This works, but we get a slew of permission denied errors. This makes it difficult to identify where the correct file is. We want to somehow suppress these - this can be done by introducing `2>/dev/null` to the command, giving us a final command of 

``` $ find / -user bandit7 -group bandit6 -type f -size 33c 2>/dev/null ```

The bandit7 password is ` HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs `

## Level 8
The password for the next level is stored in the file data.txt next to the word millionth

As we know that the password is next to the word millionth, we want to return all lines in the file that contain the world 'millionth'. This can be done by piping the contents of the file (using the `cat` command) into grep, as we have seen before. This gives a command of 

``` cat data.txt | grep millionth ```

There is only one line containing the word millionth, so we immediately find our password. It is ` cvX2JJa4CFALtqS87jk27qwqGhBM9plV `

## Level 9
Our guidance here is that the password for the next level is stored in the file data.txt and is the only line of text that occurs only once.

Immediately, by first thought was to use the uniq command, with the -u flag - this should filter a document and only return lines that occur once. As such, I ran 

``` cat data.txt | uniq -u ```

However, this caused the entire file to be printed. On reading the uniq man page, I discovered that uniq only compares adjacent lines, and that I therefore needed to sort the file first. A working solution then is 

``` cat data.txt | sort | uniq -q ```

The password to bandit 9 is ` UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR `

## Level 10
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

As we are told that the password is 'one of the few human-readable strings' we can use the command `strings`, which will return all human-readable strings in the document. We also know that the password is preceded by a string of equals signs, we can pipe the output of this command into a grep search again. This reveals the password. The final command was

``` strings data.txt | grep === ```

This reveals that the password to bandit 10 is ` truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk `

## Level 11
The password for the next level is stored in the file data.txt, which contains base64 encoded data

In order to be able to read the contents of the file, we therefore need to decode it. This can be done with the command `base64 -d data.txt`. Running this reveals the password.

The password to bandit 11 is ` IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR ` 

## Level 12
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

To decode this message, we need to shift every letter back by 13 places. This can be done using the `tr` (transliterate) command, and an appropriate filter. Again, we can use `cat data.txt | ` to pipe the file contents in. Our final command is 

``` $ cat data.txt | tr [n-za-mN-ZA-M] [a-zA-Z] ```

The password for bandit 12 is ` 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu `

## Level 13
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

I started off here by following their suggestion, and creating / navigating to /tmp/lloyd. As we know that data.txt is a hexdump, the first thing to do is apply a reverse hexdump to this file. This is done using the following command

``` xxd -r > data ```

This pipes the output of the reverse hexdump command into a new file, 'data'.
We know that this file has been compressed, but we don't know how. We can find this out by running `file data`. The output of this tells us that 'data' contains gzip compressed data. Therefore, we want to decompress it using gzip.
Simply running `gzip -d data` throws an error, as it complanes that the suffix is unknown, so we can instead use 

``` zcat -d data > data2.bin```

This decompresses the file and stores the output in data2.bin, which was the original file name. Again, we can run the `file` command to find out how this has been compressed.
This informs us that the file was compressed with bzip2. We can decompress it with 

``` bzip2 -d data2.bin```

Running this decompresses the output into a new file called 'data2.bin.out' but this is not a particularly nice name, so I renamed it to 'data3.bin' using the `mv` command. Again, I checked how it can been compressed using `file`.

These steps were repeated several times, before I eventually reached the original ASCII text file containing the password.

Password to bandit 13 is ` 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL `

## Level 14
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

Here, we are given bandit 14's private ssh key, which will allow us to log in as them. This is contained in a file called sshkey.private, in bandit 13's home directory. To pass a key in, we use the ` -i ` flag with our ssh command, like so:

``` ssh -i sshkey.private bandit14@localhost ```

This logs us in as bandit 14. From here, we can navigate to the directory given to us and simply read the password.

The password to bandit 14 is ` 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e `

## Level 15
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

The first thing we need to do is open a connection to localhost, on port 30000. This can be done with the command 

``` nc localhost 30000 ```

Once our connection is opened, we simply need to input the password to bandit 14, and the server gives back the password to bandit 15. This is ` BfMYroe26WYalil77FoDi9qh59eK5xNr `


