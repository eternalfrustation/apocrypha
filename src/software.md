# Software

## General pieces of advice for me

- Exercising regularly really does help.
- Use bash for scripts when the script will run less than 10 times in a lifetime.
- Use python for scripts when you want to be able to edit them.
- Use PlatformIO for embedded development, unless there is no mature rust HAL for a MCU.
- Prefer wgpu over platform specific APIs for graphics programming.
- Always benchmark stuff when optimizing, no exception.
- Try to avoid web development if you can, its hell incarnate.
- After some testing, Music actually slows you down, unless you are doing something that requires no thinking.
- For downloading music, 
```sh
yt-dlp -x --embed-subs --embed-metadata --embed-thumbnail --add-metadata "https://youtube.com/playlist?list=PLNPkZsUXzlEjvQemMGAnBifR4yNFO4dpb" -N 8 -I -1:-10:-1 --parse-metadata ":(?P<meta_synopsis>)"
```

## More detailed info

- [2D Rendering](./software/2d_rendering.md)
- [Audio](./software/audio.md)
- [Video](./software/video.md)
- [Operating Systems](./software/os.md)
- [Web Development](./software/web.md)
