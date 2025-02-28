# DONOTOPEN

A suspicious script file seems to be hiding something important, but it refuses to cooperate. It's obfuscated, tampered with, and demands a password. Unravel the mystery to uncover the hidden flag.

Attachment: [DONTOPEN](./DONTOPEN)

## Solution

The top of the script looks something like this:

```sh

#!/bin/bash

TMP_DIR=$(mktemp -d)
PYTHON_SCRIPT="$TMP_DIR/embedded_script.py"
CHECKSUM_FILE="$TMP_DIR/checksum.txt"

EXPECTED_CHECKSUM="g5c533c0e5e1dd82051e9ee6109144b6" 

ARCHIVE_START=$(awk '/^__ARCHIVE_BELOW__/ {print NR + 1; exit 0; }' "$0")
tail -n +$ARCHIVE_START "$0" | gzip -d > "$PYTHON_SCRIPT"


CALCULATED_CHECKSUM=$(md5sum "$PYTHON_SCRIPT" | awk '{ print $1 }')


if [ "$CALCULATED_CHECKSUM" != "$EXPECTED_CHECKSUM" ]; then
  echo "Checksum mismatch! The embedded script may have been corrupted."
  echo "Doesnt match with the MD5 checksum - a3c533c0e5e1dd82051e9ee6109144b6"
  rm -rf "$TMP_DIR"
  exit 1
fi


python3 "$PYTHON_SCRIPT"


rm -rf "$TMP_DIR"
exit 0

__ARCHIVE_BELOW__
# Apparently bytes for an archive
```

Apparently, this script does the following:

- Create a temporary dir
- Extract the archive at the end of this script into the temporary file
- Check if the checksum of the python script inside of the archive is as the expected checksum
    - if not, wipe the temp directory and exit
    - else, run the python file

When we first ran this script, we got the following:

```
Checksum mismatch! The embedded script may have been corrupted.
Doesnt match with the MD5 checksum - a3c533c0e5e1dd82051e9ee6109144b6
```

The easiest solution to this was just removing the checksum check, while doing that, i also ended up `echo' the temp directory's location and removing the `rm -rf "$TMP_DIR"` to simplify inspection

```sh
#!/bin/bash

TMP_DIR=$(mktemp -d)
PYTHON_SCRIPT="$TMP_DIR/embedded_script.py"

ARCHIVE_START=$(awk '/^__ARCHIVE_BELOW__/ {print NR + 1; exit 0; }' "$0")

tail -n +$ARCHIVE_START "$0" | gzip -d > "$PYTHON_SCRIPT"
echo $TMP_DIR
python3 "$PYTHON_SCRIPT"

exit 0
```

Running this script, we get the following output:

```
/tmp/tmp.1bSIP4aSd9
It looks like the box is locked with some kind of password, determine the pin to open the box!
What is the pin code?
```

It also opened `https://vipsace.org/` for some reason

Going into the tmp directory, `/tmp/tmp.1bSIP4aSd9` in this case, we find

```
.
└── embedded_script.py

1 directory, 1 file
```

A single python script, `embedded_script.py`.

```python
import hashlib
import requests
import webbrowser  
NOT_THE_FLAG = "flag{this-is-not-the-droid-youre-looking-for}"
# Bunch of lines with flag0 - flag999
FLAG_PREFIX = "ACE{%s}"

print("It looks like the box is locked with some kind of password, determine the pin to open the box!")
req = requests.get("http://google.com")
req.raise_for_status()

pin = input("What is the pin code?")
if pin == "ACE@SE7EN":
    print("Looks good to me...")
    print("I guess I'll generate a flag")

    req = requests.get("http://example.com")
    req.raise_for_status()

    print(FLAG_PREFIX % hashlib.blake2b((pin + "Vansh").encode("utf-8")).hexdigest()[:32])
else:
    print("Bad pin!") 
```

After removing the network requests, and entering the pin `ACE@SE7EN`, we get:

```
It looks like the box is locked with some kind of password, determine the pin to open the box!
What is the pin code?ACE@SE7EN
Looks good to me...
I guess I'll generate a flag
ACE{e2e3619b630b3be9de762910fd58dba7}
```

ACE{e2e3619b630b3be9de762910fd58dba7}

For some reason this one doesn't follow the general flag pattern of the CTF
