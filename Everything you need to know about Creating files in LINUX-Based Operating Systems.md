[Dev.to link of this article](https://dev.to/sam5epi0l/everything-you-need-to-know-about-creating-files-in-linux-based-operating-systems-52hm)

The Linux file-system considers everything as a file. From text/media/binary files and directories to hardware devices connected physically, everything is a file in linux. If it is not a file then it must be a process. In Linux, files form a tree-structure to manage the data. There are so many ways to create a file in linux, so let's look at some conventional ways to do that.


**Rules for files to exist in a Linux file-system**

1. Files are **case sensitive **(unlike Windows). So, `temp.txt`, `Temp.txt` and `TEMP.txt` all are different files.
2. Users should have permissions to create a file on the parent folder.
    1. Check permission with `ls -al` command.
    2. Make sure you are the user or from the group.
3. You can use other special characters such as blank space, but they are hard to use and it is better to avoid them.
4. filenames may contain any character except `/` , which is reserved as the separator between files and directories in a pathname. You cannot use the null character.
5. use a dot based filename extension to identify files. For example:
    1.     .sh = Shell file
    2.     .tar.gz = Compressed archive
6. Most modern Linux and UNIX limit filenames to 255 characters (255 bytes). However, some older versions of the UNIX system limit filenames to 14 characters only.
7. A filename must be unique inside its directory. For example, inside `/root` directory you cannot create a `file.txt` file and `file.txt` directory name
8. Avoid these characters from including in file names `/>&lt;|:&`
9. Enclose the file name with single-quote  `'file.txt'`.


**Little snippet of experiment with creating files in Linux:**

```
root@aaf52077a089:/# cd /root
root@aaf52077a089:~# touch '!@#$%^&*(()_+-{}[]":></?><'
touch: cannot touch '!@#$%^&*(()_+-{}[]":></?><': No such file or directory
root@aaf52077a089:~# touch '!@#$%^&*(()_+-'
root@aaf52077a089:~# touch file.txt
root@aaf52077a089:~# touch  File.txt
root@aaf52077a089:~# mkdir file.txt
mkdir: cannot create directory 'file.txt': File exists
root@aaf52077a089:~# ls -al
total 16
-rw-r--r-- 1 root root    0 Jul 16 11:19 '!@#$%^&*(()_+-'
drwx------ 1 root root 4096 Jul 16 11:20  .
drwxr-xr-x 1 root root 4096 Jul 16 11:17  ..
-rw-r--r-- 1 root root 3106 Oct 15  2021  .bashrc
-rw-r--r-- 1 root root  161 Jul  9  2019  .profile
-rw-r--r-- 1 root root    0 Jul 16 11:20  File.txt
-rw-r--r-- 1 root root    0 Jul 16 11:20  file.txt
```


**Conventional ways to create files in Linux**

We can easily create files with the default file manager (GUI). But there is no fun in it. Let’s dive into some interesting **command-line** ways to create files.



1. `touch` - Use the dedicated command to create files.
    1. Everyone's method - `touch file.txt`.
    2. Advance usage.
```
# Create a new empty file(s) or 
# change the times for existing file(s) to the current time:
touch path/to/file

# Set the times on a file to a specific date and time:
touch -t YYYYMMDDHHMM.SS path/to/file

# Set the time on a file to one hour in the past:
touch -d "-1 hour" path/to/file

# Use the times from a file to set the times on a second file:
touch -r path/to/file1 path/to/file2

# Create multiple files:
touch path/to/file{1,2,3}.txt

Credit: cheat.sh
```
2. Text editors - `nano`, `vim`, `vi`, `neovim`.
    1. These will create files at current timestamps.
    2. Syntax: `text_editor path/to/file.txt`.
3. Using the `cat` , `echo` or any other command with the `>` or `>>` operator. We can use STDOUT to create/append file.
    1. You can use simple bash tricks to use `cat`/`bat` to create files.
    2. Syntax: `cat > file.txt` , `cat >> file.txt`.

```
root@aaf52077a089:~/dir_test# cat file.txt
cat: file.txt: No such file or directory
root@aaf52077a089:~/dir_test# cat > file.txt
Creating and writing a file with cat command is so cool.
Writing on 2nd line    
^C
root@aaf52077a089:~/dir_test# cat file.txt
Creating and writing a file with cat command is so cool.
Writing on 2nd line
root@aaf52077a089:~/dir_test# cat >> file.txt
Writing on 3rd line
^C
root@aaf52077a089:~/dir_test# cat file.txt
Creating and writing a file with cat command is so cool.
Writing on 2nd line
Writing on 3rd line
root@aaf52077a089:~/dir_test# ls -al
total 12
drwxr-xr-x 2 root root 4096 Jul 16 11:55 .
drwx------ 1 root root 4096 Jul 16 11:55 ..
-rw-r--r-- 1 root root   97 Jul 16 11:57 file.txt
```



**Cool non practical methods**

1. Insert a hardware device into a Linux device. It will create a file.
2. Create files of fixed size. (10MB)

```
fallocate -l $((10*1024*1024)) file.txt
# This option doesn't use input/output overhead, the space will be allocated immediately.

truncate -s 10M file.txt
# This creates a file full of null bytes.

dd if=/dev/urandom of=ostechnix.txt bs=10MB count=1
# This command will create a non-sparse file full of null bytes.

head -c 10MB /dev/urandom > file.txt
# This command will create a non-sparse file full of null bytes.
```

Thank you so much for reading this article. Follow me for more!
