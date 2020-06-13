# Fix microphone on Ubuntu 20.04

https://bugs.launchpad.net/ubuntu/+source/kazam/+bug/1876322

```
wget https://launchpadlibrarian.net/480816598/kazam_pulse_audio_python_3_8.patch
cd /usr/lib/python3/dist-packages/kazam
sudo patch < ~/Downloads/kazam_pulse_audio_python_3_8.patch
pulseaudio/pulseaudio.py
```
