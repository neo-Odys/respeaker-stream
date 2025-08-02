# respeaker-stream

Streaming audio form a microphone(respeaker) on rpi zero 2 w to an Icecast server using ffmpeg

### Requirements
 - Raspberry pi zero 2 W
 - Seeed 2-Mic ReSpeaker Pi HAT

### Installations/Configuration
```
sudo apt update
sudo apt install ffmpeg alsa-utils icecast2 libmp3lame alsamixer
sudo nano /etc/icecast2/icecast.xml
//add at the end (before </icecast>:
<mount>
    <mount-name>/stream</mount-name>
    <password>hackme</password>
    <max-listeners>100</max-listeners>
    type>audio/mpeg</type>
</mount>
sudo systemctl enable icecast2
sudo systemctl start icecast2
```
Check if hw:0 is correct
```
arecord -l
```

### Commands
```
//no filters
ffmpeg -f alsa -i hw:0 -acodec libmp3lame -ar 16000 -ac 2 -f mp3 icecast://source:ha
ckme@localhost:8000/stream

//some filters
ffmpeg -re -f alsa -thread_queue_size 512 -i hw:0 -af "afftdn=nf=-20" -acodec libmp3lame -ar 44100 -ac 2 -b:a 128k -f mp3 icecast://source:hackme@localhost:8000/stream

//for speech
ffmpeg -re -f alsa -thread_queue_size 512 -i hw:0 -af "highpass=f=80, lowpass=f=7000, afftdn=nf=-25, dynaudnorm=f=500:g=3" -acodec libmp3lame -ar 44100 -ac 1 -b:a 96k -f mp3 icecast://source:hackme@localhost:8000/stream
```

Status Panel:
http://<rpi-ip-adress>:8000/stream
You can listen here:
http://<raspberry-pi-ip>:8000/stream

### Errors

test your mic:
```
arecord -D plughw:0 -f cd test.wav
//using winSCP you can transfer the file to your own computer
```
if the sound is too quiet, open alsamixer -> Capture/F4 -> turn up "PGA" column

```
[alsa @ 0x557d0f4550] cannot open audio device hw:0 (Device or resource busy)
hw:0: Input/output error

//Solution
pulseaudio -k
//or
lsof /dev/snd/*
kill -9 <PID of the process using the audio device>
```
