# Methodology
1. first check what scripts you can run without the permisson
   `find / -type f -perm -04000 2>/dev/null`
2. If you have any permission to the script to read any file eg,. base64 -> you can use this to read the file contents
   ` base64 /path/to/file | base64 --decode`
3. once you get any users password then you can use `sudo find . -exec /bin/sh \; -quit` this will give you the root access.
4. Next follow all other methods from the notes.
