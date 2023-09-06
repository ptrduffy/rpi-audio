# rpi-audio
Using Raspberry Pis for whole home audio

This repository has Docker container files and various configuration files for a project to create a multiroom audio system using Raspberry Pi SBCs and HiFiBerry amplifier HATs.  My goals were as follows:

  - Each room should present a Spotify and a Bluetooth connection point (so each room can have independent audio).
  - Each room should also be able to participate in a multiroom setup, where either a Spotify or Bluetooth source can stream to all participating rooms (non-participating rooms can still be independent).
  - One multiroom setup at a time is fine (I don't have a big enough house to justify multiple groups running different streams).
  - Streaming via Spotify/Bluetooth is the only initial requirement - expanding to play locally stored music later is a nice to have.

The system is based around the following components:

  - [librespot](https://github.com/librespot-org/librespot) - a Spotify Connect endpoint allowing any Spotify client to stream music to and control the device
  - [snapcast](https://github.com/badaix/snapcast) - a multiroom client-server audio player

The system is implemented using multiple Docker containers to provide isolation between components.  The various containers used are:

  - A librespot container - I run one of these on each RPi, so that each room can have a different Spotify stream playing, when so desired
  - A snapclient container - again, one is run on each RPi, so that each room can be part of a multiroom group when desired
  - A snapserver container - this runs on just one of the RPis, with routing of audio to clients controlled by the snapdroid app

Each of the player containers (librespot and snapclient) uses the alsa and pulseaudio Linux sound frameworks.  It's possible to have a similar setup with just alsa, but I used pulseaudio for two reasons: firstly, I wanted to have a bluetooth endpoint on each RPi, and this was much easier to set up with pulseaudio; secondly, on one of my RPis I wanted to connect a subwoofer, and I was able to configure pulseaudio to route the audio stream through both the HiFiBerry strereo amp **and** the RPi headphone socket simultaneously, with the latter feeding a powered sub.
