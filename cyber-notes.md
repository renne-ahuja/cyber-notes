OverTheWire Bandit — Linux & Cybersecurity Notes
Learning track: Linux Fundamentals and Cybersecurity
Platform: OverTheWire Bandit
Coverage: Levels 0 → 4
________________________________________
1. Level 0 → Level 1: SSH and Reading Files
Objective
Connect to the Bandit server and retrieve the password for bandit1.
Command 1: SSH- secure shell protocol connection
ssh bandit0@bandit.labs.overthewire.org -p 2220
Explanation
•	ssh: Starts a Secure Shell connection.
•	bandit0: Username for the remote account.
•	bandit.labs.overthewire.org: Remote server hostname.
•	-p 2220: Specifies port 2220.
SSH provides encrypted communication between a client and a remote server.
Command 2: Check the current user
whoami
Displays the username of the current session.
Example output:
bandit0
Command 3: Display the current directory
pwd
pwd stands for Print Working Directory. It displays the absolute path of the current directory.
Example output:
/home/bandit0
Command 4: List files
ls
Lists files and directories in the current directory.
Command 5: Read a file
cat readme
cat displays the contents of a file. In this level, the readme file contains the password for bandit1.
Key takeaways
•	SSH allows secure remote login.
•	whoami identifies the current user.
•	pwd identifies the current working directory.
•	ls lists directory contents.
•	cat reads file contents.
Cybersecurity relevance
These commands are useful for remote administration, navigating Linux systems, and investigating files during authorized security assessments.
________________________________________
2. Level 1 → Level 2: Special Filenames
Objective
Read a file named - to obtain the password for bandit2.
The problem
A standalone hyphen (-) is interpreted by many command-line programs as a special argument, often representing standard input or output.
Consequently, this command may wait for input rather than read the intended file:
cat -
Correct command
cat ./-
Explanation
•	cat: Displays file contents.
•	./: Refers to the current directory.
•	-: The actual filename.
The explicit relative path tells the shell and command to treat the argument as a file path.
Another method, for commands that support the end-of-options marker, is:
cat -- -
Here, -- indicates that subsequent arguments should not be interpreted as command options.
Important concepts
Relative path: A path interpreted relative to the current directory.
Standard input (stdin): The input stream a program reads from.
End-of-options marker (--): A convention supported by many commands to mark the end of options.
Key takeaway
When a filename has a special meaning, use an explicit path such as ./- to avoid ambiguity.
Cybersecurity relevance
Unusual filenames occur in real systems and security challenges. Handling them correctly is important for reliable scripts and file investigations.
________________________________________
3. Level 2 → Level 3: Filenames Containing Spaces
Objective
Read a file in the bandit2 home directory to retrieve the password for bandit3.
The problem
The filename shown in the terminal was:
--spaces in this filename--
Notice the two hyphens at the beginning and the end.
The command must match the filename exactly.
Correct command
cat -- "--spaces in this filename--"
Explanation
•	cat: Displays file contents.
•	--: Ends command-option processing.
•	Double quotes: Keep the spaces within one filename argument.
•	--spaces in this filename--: The exact filename.
Alternative method: Escape spaces
For a filename without the leading and trailing hyphens, such as my notes.txt, you can use:
cat my\ notes.txt
The backslash (\) escapes each space so that the shell treats the filename as one argument.
Debugging with ls -la
If a command returns No such file or directory, inspect the directory:
ls -la
Check for:
•	Leading or trailing characters.
•	Spaces in the filename.
•	Differences in capitalization.
•	The correct current directory.
Key concepts
Shell: A program that interprets commands and passes arguments to programs.
Quoting: Groups text so spaces and certain special characters are handled correctly.
Escaping: Prevents a character from being interpreted in its usual special way.
Key takeaway
Always use the exact filename. Quotes handle spaces, while -- can prevent filenames from being interpreted as command options.
Cybersecurity relevance
Correctly handling filenames is important when writing shell scripts, inspecting suspicious files, and automating security investigations.
________________________________________
4. Level 3 → Level 4: Hidden Files
Objective
Find the password for bandit4 in a hidden file inside the inhere directory.
Command 1: Enter the directory
cd inhere
cd stands for Change Directory. It changes the current working directory.
Command 2: List all entries
ls -la
Explanation
•	ls: Lists directory contents.
•	-l: Shows a detailed listing.
•	-a: Includes hidden entries.
The detailed listing displays information such as file permissions, owner, group, size, and modification time.
File discovered
The terminal showed:
...Hiding-From-You
The filename starts with three dots. It is a valid filename and is treated as hidden because it begins with a dot.
Command 3: Read the file
cat ...Hiding-From-You
This displays the file's contents, which contain the password for bandit4.
Understanding hidden files
In Linux, filenames beginning with a dot are normally omitted from the output of a standard ls command.
Examples:
•	.bashrc
•	.profile
•	.config
Use ls -a or ls -la to display hidden entries.
Important: Hidden does not mean encrypted or secure. File permissions determine whether an account can access a file.
Understanding Linux permissions
Example permission string:
-rw-r-----
Part	Meaning
-	Regular file
rw-	Owner can read and write
r--	Group can read
---	Others have no permissions
The file discovered in the challenge was owned by bandit4 and belonged to the bandit3 group.
Key takeaway
Use ls -la when you suspect a file exists but cannot see it with a normal ls command.
Cybersecurity relevance
Security analysts inspect hidden files and file permissions to investigate system configurations, identify exposed information, and understand access controls.
________________________________________
5. Linux Commands Learned So Far
Command	Purpose
ssh	Connect to a remote system securely
whoami	Display the current username
pwd	Display the current directory
ls	List visible directory contents
ls -a	List all entries, including hidden ones
ls -la	Show all entries in detailed format
cd	Change the current directory
cat	Display file contents
exit	Close the current shell or SSH session
Quick revision examples
whoami
pwd
ls
ls -la
cd inhere
cat readme
cat ./-
cat -- "--spaces in this filename--"
cat ...Hiding-From-You
exit
Remember that the last three file-reading commands are specific to the filenames encountered in these challenges.
________________________________________
6. Troubleshooting Notes
Error: No such file or directory
Possible causes:
•	The filename is incorrect.
•	The current directory is wrong.
•	The filename contains spaces or special characters.
•	Leading or trailing characters were missed.
Useful commands:
pwd
ls -la
ls -lb
ls -lb displays filenames with special characters escaped, helping you inspect their exact names.
Error: Permission denied
The account may not have permission to read the file or access the directory. Check the permissions and ownership displayed by ls -l.
Do not assume that every permission error should be fixed by changing permissions; in a security challenge, the intended task is to use the access provided.

# OverTheWire Bandit — Levels 4 to 7

## Level 4 → Level 5: Finding Human-Readable Data

### Objective
Find the human-readable file among multiple files in the `inhere` directory.

### Commands Used
```bash
cd inhere
ls -la
file ./*
```

After identifying the file reported as human-readable text, read it using its exact filename. For example, if the output identifies `-file07`:

```bash
cat ./-file07
```

### Concepts Learned
- `file` identifies the type of data stored in a file.
- `./*` matches directory entries in the current directory, including filenames beginning with a hyphen.
- `cat` displays the contents of a file.
- A filename beginning with `-` can be accessed safely using an explicit path such as `./-file07`.

### Key Takeaway
When many files look similar, use `file` to identify the correct file instead of opening them one by one.

---

## Level 5 → Level 6: Finding a File by Properties

### Objective
Find the password file somewhere on the server. The file has these properties:
- Human-readable
- 1033 bytes in size
- Not executable

### Commands Used
```bash
cd inhere
ls -la
find . -type f -size 1033c
```

To search specifically for a non-executable regular file of the required size:

```bash
find . -type f -size 1033c ! -executable
```

Inspect the candidate file with:

```bash
file ./candidate
cat ./candidate
```

Replace `./candidate` with the actual path returned by `find`.

### Concepts Learned
- `find` searches for files and directories matching specified conditions.
- `-type f` selects regular files.
- `-size 1033c` searches for files exactly 1033 bytes long; `c` means bytes.
- `! -executable` excludes files that are executable by the current user.
- Multiple conditions can be combined to narrow a search.

### Key Takeaway
Use file properties such as size, type and permissions to narrow down search results efficiently.

---

## Level 6 → Level 7: Searching Across the Server

### Objective
Find the password file somewhere on the server with these properties:
- Owned by user `bandit7`
- Owned by group `bandit6`
- Exactly 33 bytes in size

### Commands Used
```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Inspect the resulting path:

```bash
file /path/to/candidate
cat /path/to/candidate
```

Replace `/path/to/candidate` with the exact path returned by `find`.

### Concepts Learned
- `find /` searches from the root directory.
- `-user bandit7` matches files owned by the specified user.
- `-group bandit6` matches files owned by the specified group.
- `-size 33c` matches files exactly 33 bytes long.
- `2>/dev/null` redirects error messages to `/dev/null`, hiding permission-denied messages and other standard error output.

### Key Takeaway
Linux file ownership and permissions help determine which files match a search. Combining several conditions makes large filesystem searches more manageable.

---

## Level 7 → Level 8: Searching for a Specific String

### Objective
Find the password in the file `/data.txt`. The password is on the same line as the word `millionth`.

### Commands Used
```bash
ls
grep "millionth" data.txt
```

An alternative is:

```bash
grep -n "millionth" data.txt
```

The `-n` option displays the matching line number.

### Concepts Learned
- `grep` searches text for a matching pattern.
- Quotation marks make the search term explicit.
- `-n` displays line numbers alongside matching lines.
- Searching for a known keyword is often faster than reading a large file manually.

### Key Takeaway
Use `grep` when you know a word or pattern that appears near the information you need.

---

## Commands Reference

| Command | Purpose |
|---|---|
| `file` | Identify file types |
| `find` | Search for files and directories |
| `-type f` | Match regular files |
| `-size 1033c` | Match files exactly 1033 bytes long |
| `-user` | Filter by file owner |
| `-group` | Filter by group ownership |
| `! -executable` | Exclude files executable by the current user |
| `2>/dev/null` | Hide standard error output |
| `grep` | Search text for a pattern |
| `grep -n` | Search text and show matching line numbers |

