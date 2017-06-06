Installation Instructions
=========================

1. Install the regular OBS package by running obs-mac-19.0.2-installer.pkg and following the instructions
2. Install the iShowU Audio Capture.pkg to install an audio driver allowing you to capture your desktop audio.
3. Move the OBS.app in the DMG image provided into your Applications folder overwriting the one already present to get Beam/Mixer FTL support.
4. Install Homebrew from https://brew.sh/
5. Open a Terminal and run:
```
brew install qt5
brew install jansson
brew install x264
brew install ffmpeg --with-fdk-aac --with-sdl2 --with-freetype --with-libass --with-libvorbis --with-libvpx --with-opus --with-x264 --with-x265
```

Configuration
=============

For desktop audio capture:
1. Open Audio MIDI Setup from Applications > Utilities.
2. Click the + button in the bottom left corner and select "Create Multi-Output Device".
3. Check "Built-in Output" or your primary audio device connected to your speakers (this may already be checked) and "iShowU Audio Capture".
4. Make sure "Master Device" is set to "Built-in Output" or your primary audio device connected to your speakers and "Sample Rate" is set to "48.0 kHz", check "Drift correction" for the "iShowU Audio Capture" device and uncheck it for "Built-in Output" (or your audio device connected to your speakers).
5. Right-click (or control-click) the device you just created and select "Use this device for sound output".

6. Open OBS.
7. Click on Settings to open the Settings window, select Audio.
8. For "Mic/Auxiliary Audio Device" select "iShowU Audio Capture"
9. For "Mic/Auxiliary Audio Device 2" select "Built-in Microphone" or your other microphone or audio recording device you would like to use.

Configure everything else in OBS as usual (i.e. like you would on Windows).