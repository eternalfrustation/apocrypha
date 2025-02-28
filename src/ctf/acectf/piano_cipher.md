# Pipher - Piano Cipher
Back in my school days, we were required to pick a "Leisure Time Activity" as part of the curriculum. Four days a week, we had to attend those classes and learn whatever we chose. I opted for the piano—or more specifically, the keyboard.

From junior school through high school, I gradually improved. Over time, I moved beyond simply memorizing notes and developed the ability to judge sounds and map notes instinctively, even when hearing a song for the first time. When I discovered this skill, it felt incredible.

But this joy was short-lived. My musical journey ended when I entered high school, as academics took precedence. Later, I pursued my bachelor's degree far from home, leaving my piano behind. It's a decision I've often regretted. Now, all I have are fond, fleeting memories of those starry-eyed days as a kid.

In honor of that time, I've created a cipher called the Pipher. There's a bug in it, though—every 6th character of the plaintext leaks. So, yeah, it’s still a work in progress.

Attachment: File named cipher.txt containing:

```
Ciphertext - DC# DD# DF DD# EC '70' G#B CE F#C FC# C#C# '104' C#A FC# F#A# C#A C#A '108' CF AF# C#C FC# CE '102' FC# C#A# FC# GA# CE '112' FC# C#B C#C# C#A# GC '125'
```

## Solution

Having only rudimentary knowledge about music theory, I just assumed that notes represented numbers and started from there,

After a bit of googling around, i stumbled upon [an image explaining piano scales](https://www.piano-keyboard-guide.com/wp-content/uploads/2018/12/Piano-Keys-and-Notes-Piano-Keyboard-Diagram-1-1024x576.jpg)

From here, i just wrote a script which mapped the above cipher text into integers based on this and then to chars, strung them together and that was that:

```py
#!/usr/bin/env python3

input = "DC# DD# DF DD# EC '70' G#B CE F#C FC# C#C# '104' C#A FC# F#A# C#A C#A '108' CF AF# C#C FC# CE '102' FC# C#A# FC# GA# CE '112' FC# C#B C#C# C#A# GC '125'"

map = ["A", "A#", "B", "C", "C#", "D", "D#", "E", "F", "F#", "G", "G#"]

output = []

print(len(input.split()))

for char in input.split():
    current = char
    if current.startswith("'"):
        current = current.strip("'")
        output.append(int(current))
        continue
    for i in range(len(map) - 1, -1, -1):
        if current.startswith(map[i]):
            output.append(i + 1)
            current = current[len(map[i]) :]
            break

    for i in range(len(map) - 1, -1, -1):
        if current.startswith(map[i]):
            output[len(output) - 1] = output[len(output) - 1] * 10 + i + 1
            break

print(len(output))

print("".join([chr(x) for x in output]))
```

Running this gave the output of:

```
36
36
ACECTF{0h_7h3_f33l16_0f_4_p0p_574r}
```

But there was a problem, the site didn't accept this flag, i ended up one of the organizer via discord, and he told me there was a typo.

I figured, that `0h_7h3_f33l16_0f_4_p0p_574r` is probably `oh the feelig of a pop star` 

i manually added an n in f33l1n6 and the site accepted it, idk if this was how it was supposed to be solved, but hey, it worked


ACECTF{0h_7h3_f33l1n6_0f_4_p0p_574r}
