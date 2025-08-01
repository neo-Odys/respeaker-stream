# respeaker-stream

Streaming audio form a microphone(respeaker) on rpi zero 2 w to an Icecast server using ffmpeg

### Requirements
 - Raspberry pi zero 2 W
 - respeaker

### Installations
```
sudo apt update
sudo apt install ffmpeg alsa-utils icecast2 libmp3lame
```


ffmpeg -f alsa -i hw:0 -acodec libmp3lame -ar 16000 -ac 2 -f mp3 icecast://source:ha
ckme@localhost:8000/stream

ffmpeg -re -f alsa -thread_queue_size 512 -i hw:0 -af "afftdn=nf=-20" -acodec libmp3lame -ar 44100 -ac 2 -b:a 128k -f mp3 icecast://source:hackme@localhost:8000/stream

ffmpeg -re -f alsa -thread_queue_size 512 -i hw:0 -af "highpass=f=80, lowpass=f=7000, afftdn=nf=-25, dynaudnorm=f=500:g=3" -acodec libmp3lame -ar 44100 -ac 1 -b:a 96k -f mp3 icecast://source:hackme@localhost:8000/stream

http://192.168.0.115:8000/stream
