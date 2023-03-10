# Distortioner
![Distorted cat](avatar.jpg)

Telegram bot for distorting pictures, stickers, voice messages and GIFs using Content Aware Scale.
Currently resides here: [@distortionerbot](https://t.me/distortionerbot)

## Usage
1. You'll need to install [ffmpeg](http://ffmpeg.org) and [ImageMagick](http://www.imagemagick.org/) with liquid-rescale enabled. For that you'll need to install [liblqr](https://github.com/carlobaldassi/liblqr) and glib-2.0, then [compile from source](https://imagemagick.org/script/install-source.php) (using AppImage might work too)
2. Create a bot with [@BotFather](https://t.me/BotFather), then set up a `DISTORTIONER_BOT_TOKEN` environment variable.
3. Set up `DISTORTIONER_ADMIN_ID` variable (needed to use `/daily`, `/weekly`, `/monthly` commands to monitor bot usage and to use video stickers distortions)
4. After that grab `distortioner` from releases or compile using `go build` command.

## Docker support
Fill out your bot token in distortioner.env (and your admin ID if you wish to monitor stats), then launch as usual:
```Bash
sudo docker run -d --restart unless-stopped \
   --name distortioner \
   --env-file distortioner.env \
   -v /path/to/database/directory:/app/data
   ghcr.io/graynk/distortioner:latest
```

With Podman:
```Bash
podman run -d \
   --label "io.containers.autoupdate=image" \
   --name distortioner \
   --env-file distortioner.env \
   -v /path/to/database/directory:/app/data
   ghcr.io/graynk/distortioner:latest
cd ~/.config/systemd/user/
podman generate systemd --new --files --name distortioner
systemctl enable --user container-distortioner.service
loginctl enable-linger $USER
```

## What gets collected
The bot _technically_ is able to see __all__ messages in the group due to limitations imposed by Telegram 
(I've tried making it work with privacy mode enabled, but this renders `/distort` command useless).

However, none of it is stored or analysed in any way. The only thing I collect is chat IDs for the stats 
(so if you use the bot in chat group - I store only chat group ID).

In addition to that, if the bot fails to distort some piece of media, it will not get deleted as well, to help me debug such cases
(your user ID is not tied to the file itself)

## TODO
* Distort animated stickers
* Make distortion increase throughout the video
* For stickers create and maintain a separate sticker pack for ease of use
* Localization, refactoring, tests
