# Bash/Shell Scripting

## Arrays

- [http://www.thegeekstuff.com/2010/06/bash-array-tutorial](http://www.thegeekstuff.com/2010/06/bash-array-tutorial)

## Make Directory

```bash
mkdir directory_name
# -p flag will make directory if it does not already exist
mkdir -p directory_name
```

## CURL

```bash
curl http://pathtofile.xml -o ~/Desktop/output_filename.xml
```

## SCP

The basic format of the command:

```bash
scp [options] original_file destination_file
```

To format the remote portion:

    user@server:path/to/file

*Note: to copy whole directories use the [-r] flag*

## Symbolic Link

```bash
ln -s <path_to_file_that_should_be_linked> .
```

Symbolic link to all files in a folder (OSX)

```bash
ln -s <path_to_file_that_should_be_linked/*> .
```

To check if a symbolic link exists before creating:

```bash
cdn=../cdn
path=../build
files=( "docs" "fonts" "media" "ui")

for i in "${files[@]}"
  do
    if [[ -h "$path/$i" ]]; then echo "{$i} link exists"; else ln -s "$cdn/$i" "$path/$i"; fi
done
```

This will loop through the defined `path` and check each of the items in the `files` array to see if they exist as a symbolic link. If they do not exist, the script will create the symlinks.

## File copy maintaining Date/Owner info

```bash
cp -a path/filename
```

## Open files in specific apps

[http://hints.macworld.com/article.php?story=2004012218171997](http://hints.macworld.com/article.php?story=2004012218171997)

```bash
open -a "Adobe Photoshop 7.0" foo.jpg
```

## Directory compare and txt list output

Compare directories and export txt list of differences:

```bash
diff -qr dirA dirB | grep -v -e 'DS_Store' -e 'Thumbs' | sort > diffs.txt
```

## List contents of zipped file

To list contents of zipped file:

```bash
unzip -l filename.zip
```

## Copy

```bash
cd /destination of copy/
cp -R /directory_files to copy .
```

### Copy With Preservation

```bash
$ cp -rp /source /destination
#
# cp manpage details
-p    same as --preserve=mode,ownership,timestamps

--preserve[=ATTR_LIST]
        preserve the specified attributes (default: mode,ownership,timestamps),
        if possible additional attributes: context, links, xattr, all
```

## Batch Rename

```bash
for file in *.zipd; do mv "$file" "${file%zipd}zip"; done
```

## Batch Remove Spaces In File Names

```bash
#! /bin/sh
for n in *
do
OldName=$n
#NewName=`echo $n | tr -d " "`
NewName=`echo $n | tr -s " " "-"`
echo $NewName
mv "$OldName" "$NewName"
done
```

To run this script, `cd` into the destination directory and then execute. Example:

```bash
cd directory/my-files/
~/Documents/./remove-spaces *
```

## Create Executable Shell Script

```bash
$ ls
myscript.sh
$ mv myscript.sh myscript
$ chmod 755 myscript
# To use the script from the command line:
$ ./myscript
```

## Find & Remove Files Based On Wildcard String

_*NEEDLE*_ represents the wildcard string value that is being searched for in the file names.

```bash
find . -maxdepth 1 -name "*NEEDLE*" -print
find . -maxdepth 1 -name "*NEEDLE*" -exec rm {} \;
```

## Show files by edit date

List files edited in the past day

```bash
find . -type f -newermt 2011-06-07
find . -type f -newermt 2011-06-06 ! -newermt 2011-06-07 | sort > ~/Desktop/list.txt
```

## Outgoing IP address

```bash
curl ifconfig.me
```

## Network Commands

[http://www.computerhope.com/](http://www.computerhope.com/)

## Extract files from multiple folders

```bash
find path/to/extract/from -name *.mp3 -exec cp {} /path/to/destination \;
```

## sftp / ssh

To access a remote server without the use of a public key use:

```bash
ssh -o "PubkeyAuthentication no" user@server
sftp -o "PubkeyAuthentication no" user@server
```

## Get User Input

```bash
echo "Type something and then hit [ENTER]"
read userinput
```

## Does directory exist?

```bash
if [ -d "$dir" ]
then
  echo "$dir directory already exists!"
else
  echo "$dir directory not found!"
fi
```

## Some helpful ftp commands:

- pwd:    print working directory of remote host
- lpwd:   print working directory of local host
- cd: change directory of remote host
- lcd:    change directory of local host
- ls: list directory on the remote host
- lls:    list directory on the local host
- mkdir:  make directory on remote host
- lmkdir: make directory on local host
- get:    receive file from remote host to local client
- put:    send file from local client to remote host
- help:   display help text


## Reformat Text As A Long List of Words

```bash
tr [:cntrl:] " " <filename | tr -s " " >new.filename
```

- [http://www.linuxforums.org/forum/ubuntu-linux/153458-scipt-add-carriage-return-text-after-number-words.html](http://www.linuxforums.org/forum/ubuntu-linux/153458-scipt-add-carriage-return-text-after-number-words.html)

## Insert a new line every ten words

```bash
xargs -n10 < file
# output to a new file
xargs -n10 < new.file
```

- [http://stackoverflow.com/questions/15979802/how-to-insert-a-newline-n-after-x-numbers-of-words-with-awk-or-sed](http://stackoverflow.com/questions/15979802/how-to-insert-a-newline-n-after-x-numbers-of-words-with-awk-or-sed)

## Package Assets

```bash
#!/bin/bash
cd ${0%/*}

# Package `build` contents for delivery.
read -r -p "Package for delivery? (did you --production flag everything?) [Y/n] " response
case $response in
    [yY][eE][sS]|[yY])
        echo 'Copying files...'
        cp -r ../build ~/Desktop/.
        cd ~/Desktop/
        mv build package_name
        echo 'Done.'

        echo 'Zipping files...'
        zip -r9X package_name.zip package_name
        echo 'Done.'

        read -r -p "Delete prep directory? This will leave the .zip file but remove the temporary source. [Y/n] " response
        case $response in
          [yY][eE][sS]|[yY])
            echo 'Cleaning up...'
            rm -r package_name
            ;;
        esac

        cd -
        echo 'Packaging complete.'
        open ~/Desktop/
        ;;
    *)
        # do_something_else
        ;;
esac
```
