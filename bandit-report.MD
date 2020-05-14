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


