# Heroku-Rclone-21vianet. Heroku-Rclone 世纪互联版
Using Rclone with 21vianet mod and Aria2, even UNRAR easily on Heroku.<br>
在 Heroku 上轻松运行 Rclone、Aria2，甚至是 UNRAR。

Only useing Aria2 and dislike command terminal? Try this [Heroku AriaNG 21vianet](https://github.com/szyha/heroku-ariang-21vianet)<br>
仅仅想用 Aria2 下载并且不喜欢命令行？试试这个 [Heroku-AriaNG 世纪互联版](https://github.com/szyha/heroku-ariang-21vianet)

## Deploy
1. Create new app

```
heroku create myapp -b https://github.com/szyha/heroku-rclone-21vianet.git
heroku git:clone -a myapp
cd myapp
heroku buildpacks:add --index 2 https://github.com/jonathanong/heroku-buildpack-ffmpeg-latest
heroku buildpacks:add --index 3 heroku-community/apt
# optional for nano editor
heroku buildpacks:add --index 4 https://github.com/velizarn/heroku-buildpack-nano
# or useing existed app
heroku buildpacks:set https://github.com/szyha/heroku-rclone-21vianet.git -a myapp
heroku buildpacks:add --index 2 https://github.com/jonathanong/heroku-buildpack-ffmpeg-latest -a myapp
heroku buildpacks:add --index 3 heroku-community/apt -a myapp
# optional for nano editor
heroku buildpacks:add --index 4 https://github.com/velizarn/heroku-buildpack-nano -a myapp
```

2. Setup Rclone by following [Rclone Docs](https://rclone.org/docs/), Chinese users can setup with 21vianet patch to connect OneDrive by 21vianet.<br> 
You can find your config from there:

```
Windows: %userprofile%\.config\rclone\rclone.conf
Linux: $HOME/.config/rclone/rclone.conf
```
Optional: Using service account setup with [Gclone](https://github.com/donwa/gclone) to break Google Drive 750GB limit, or easier connect to folder or Team Drive by destination ID. Create a new folder, such as `/accounts/`, upload your json in it. Open rclone config and edit `service_account_file_path = /app/accounts/` as the json paths.

Rclone with 21vianet patch and Gclone mod provided by xhuang.

3. Go to `myapp` directory, copy `rclone.conf` and winrar registraton key `.rarreg.key` (optional) then commit the change.

```
cd myapp
git add .
git commit -am "add config"
git push heroku master
```

## Usage
### Open Terminal
```
cd myapp
heroku run bash
# or
heroku run bash --a myapp
```

### Rclone
Learn more from [Rclone Docs](https://rclone.org/commands/) and [Gclone Docs](https://github.com/donwa/gclone)

**Upload file to Google Drive**
```
# The usual way
rclone -P copy local_dir Google:remote_dirpath

# The way that like using gclone
rclone -P copy local_dir Google:{Destination_ID}
```
`-P` mean print progress in real time

### Aria2
Learn more from [Aria2c Docs](http://aria2.github.io/manual/en/html/aria2c.html)

**Download a file**
```
aria2c -x4 http://host/file.rar
```
`-x4` mean download using 4 connection

### Megatools
Learn more from [Megatools Docs](https://megatools.megous.com/man/megatools.html)

**Download a file**
```
megatools dl https://mega.nz/url
```

### youtube-dl
Learn more from [youtube-dl Docs](https://github.com/ytdl-org/youtube-dl/blob/master/README.md#options)

**Download YouTube video**
```
youtube-dl https://www.youtube.com/watch?v=jNQXAC9IVRw
```

### irssi
Learn more from [irssi Docs](https://irssi.org/documentation/)

**Open irssi**
```
irssi
```

**Connect to irc.rizon.net**
```
/connect irc.rizon.net
```

**Change nick**
```
/nick your_new_nick
```

**Join to channel**
```
/join name_channel
```
### UNRAR
Learn more from [UNRAR Docs](https://pypi.org/project/unrar/)

**Extract `.rar` or `.zip` Compressed file**
```
# To current directory
unrar e file.rar/zip

# With full path
unrar x file.rar/zip
```

## Tips

**Speed up upload**

If you want to upload many files smaller than 8mb increase only `--transfers` option
```
rclone -v --transfers=16 --drive-chunk-size=16384k --drive-upload-cutoff=16384k copy local_dir gdrive_config:remote_drive_dir
 ```
`--transfers=N`  number parallel of connection. `default: 4`

` --drive-chunk-size=N` if file bigger than this size it will splits into multiple upload, increase if you want better speed. `default: 8192k or 8mb`

`--drive-upload-cutoff=N` should be same with chunk size

`-v` option to view upload progress stats 
