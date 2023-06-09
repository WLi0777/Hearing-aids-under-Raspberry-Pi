
# Hearing aids research under Raspberry Pi

_2022-04_

_To provide intelligent voice assistants, sound source localization and tracking functions as well as basic sound gain and noise filtering functions._



<details id=0 open>
<summary><h2>About</h2></summary>

Existing hearing aid auditory frameworks provide basic sound amplification and crude noise filtering capabilities, limiting their growth in the existing AI technology market. 
This research results in increased hearing aid processing power and programmability for more sophisticated processing strategies to improve the quality of sound delivered to the impaired ear.

- ***What is included***: [Raspberry Pi 4B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/), [ReSpeaker 4-MIC Array](https://wiki.seeedstudio.com/ReSpeaker_4_Mic_Array_for_Raspberry_Pi/), Headphone (Output), Screen, Remote controller
- ***What is new***: The core technical difference between the hearing aids studied in this experiment and ordinary hearing aids is the processing method inside the chip for the sound signals collected in by the microphone. 
- ***What will build***: Raspberry Pi-based hearing aids can be extended with software-controlled features to include Alexa voice assistant, sound source location and tracking, noise reduction, binaural processing, reverberation cancellation. 

<p align="center">
<img alt="Deliverable" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/Deliverables.png width=876 hight=412>
 

</details>

<details id=1>
<summary><h2>Step 1: Raspbian and ReSpeaker 4-Mic Array setup</h2></summary>
  
### :floppy_disk: Burn Raspbian on SD card (MacOS)

1. Go to [Raspberry Pi OS](https://www.raspberrypi.com/software/), obtain and install the .img file for Raspberry Pi Imager.
2. Go to [Index of Raspbian](https://downloads.raspberrypi.org/raspbian/images/), select 'raspbian-2020-02-14', download '2020-02-13-raspbian-buster.zip'.

   > The reason for not downloading the latest version is that ReSpeaker 4-Mic Array can only be adapted to the 2020-02-13 version of Raspbian.

3. Upload the file of Pi OS to Raspberry Pi Imager. Make sure to check the target location of the SD Card that is located on the home page of Raspberry Pi OS Imager. Click 'WRITE' to install.

 <p align="center">  
 <img alt="Imager" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/Raspberry%20Imaging.png width=606 hight=238>

&nbsp;
###  :sound: ReSpeaker 4-Mics Pi HAT setup

1. Download the Seeed voice card source code

    ```
    sudo apt-get update
    git clone https://github.com/Seeed-Projects/seeed-voicecard.git
    cd seeed-voicecard
    sudo ./install.sh --compat-kernel
    reboot
    ```

2. Check that the sound card 



    ```
    cd seeed-voicecard
    arecord -L
    ``` 
    

    The details of soundcard should show like this:




    ```
    pi@raspberrypi:~ $ cd seeed-voicecard
    pi@raspberrypi:~/seeed-voicecard $ arecord -L
    null
        Discard all samples (playback) or generate zero samples (capture)
    jack
        JACK Audio Connection Kit
    pulse
        PulseAudio Sound Server
    default
    playback
    ac108
    usbstream:CARD=b1
        bcm2835 HDMI 1
        USB Stream Output
    usbstream:CARD=Headphones
        bcm2835 Headphones
        USB Stream Output
    sysdefault:CARD=seeed4micvoicec
        seeed-4mic-voicecard, bcm2835-12s-ac10x-codeco ac10x-codec@-0
        Default Audio Device
    dmix:CARD=seeed4micvoicec,DEV=0
        seeed-4mic-voicecard, bcm2835-12s-ac10x-codeco ac10x-codec@-0
        Direct sample mixing device
    dsnoop:CARD=seeed4micvoicec,DEV=0
        seeed-4mic-voicecard, bcm2835-12s-ac10x-codeco ac10x-codec@-0
        Direct sample snooping device
    hw:CARD=seeed4micvoicec,DEV=0
        seeed-4mic-voicecard, bcm2835-12s-ac10x-codeco ac10x-codec@-0
        Direct hardware device without any conversions
    plughw:CARD=seeed4micvoicec,DEV=0
        seeed-4mic-voicecard, bcm2835-12s-ac10x-codeco ac10x-codec@-0
        Hardware device with all software conversions
    usbstream:CARD=seeed4micvoicec
        seeed-4mic-voicecard
        USB Stream Output
    ```



3. Adjust the microphone volume

    ```
    alsamixer
    ``` 
  
<p align="center">
<img alt="AlsaMixer" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/AlsaMixer.png width=569 hight=340>


4. Install audacity for recording
  
    ```
    sudo apt update
    sudo apt install audacity 
    audacity
    ``` 
  
<p align="center">
<img alt="audacity" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/audacity.png width=510 hight=376>
  

5. Raspberry Pi configuration setup 
     Set Headphone as output, SPI SSH and I2C to be enabled.
 
6. Check number
 
    :pushpin: Voicecard represents as **hw:2,0**
&nbsp;
 
    ```
    arecord -l
    ``` 

 
    ```
    pi@raspberrypi:~ $ arecord -l
    **** List of CAPTURE Hardware Devices ****
    card 2: seeed4micvoicec [seeed-4mic-voicecard], device 0: bcm2835-i2s-ac10x-code
    c0 ac10x-codec0-0 [bcm2835-i2s-ac10x-codec0 ac10x-codec0-0]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    ``` 

     :pushpin: Headphone represents as **hw:1,0**
&nbsp;    

    ```
    aplay -l
    ``` 

    ```
    pi@raspberrypi:~ $ aplay -l
    **** List of PLAYBACK Hardware Devices ****
    card 0: b1 [bcm2835 HDMI 1], device 0: bcm2835 HDMI 1 [bcm2835 HDMI 1]
      Subdevices: 4/4
      Subdevice #0: subdevice #0
      Subdevice #1: subdevice #1
      Subdevice #2: subdevice #2
      Subdevice #3: subdevice #3
    card 1: Headphones [bcam2835 Headphones], device 0: bcm2835 Headphones [bcm2835 Headphones]
      Subdevices: 4/4
      Subdevice #0: subdevice #0
      Subdevice #1: subdevice #1
      Subdevice #2: subdevice #2
      Subdevice #3: subdevice #3
    ``` 
   

    :pencil2: Reset the variables in the default sound card


    ```
    sudo nano /home/pi/.asoundrc
    ``` 
 
    ```
    pcm.!default {
      type asym
      playback.pcm {
        type plug
        slave.pcm "hw:1,0"
      }
      capture.pcm {
        type plug
        slave.pcm "hw:2,0"
      }
    }
    
    pcm.output {
      type hw
      card 1
    }
 
    ctl.!default {
      type hw
      card 0
    }
    ``` 
 
  
    :keyboard: Enter **Ctrl+X**, press **Y**, and **Enter** to exit
 

 7. Record and display
 
    :open_file_folder: Create a demo under home / PI Wav recording file, say 3 seconds, it will start to record
 

    ```
    arecord -d 3 demo.wav
    ```
 
    :sound: To display the demo.wav:
 

 
    ```
    aplay demo.wav
    ```


8. APA102 LED

    > This section is a simple test to see if the LEDs on the sound card are working.

    :pushpin: Install spidev gpiozero and pixel
&nbsp;
 
    ```
    pip install spidev gpiozero
    git clone --depth 1 https://github.com/respeaker/pixel_ring.git cd pixel_ring
    pip install -U -e .
    cd examples/
    ``` 

    :runner: Run the demo to see LEDs blink
&nbsp;
 
    ```
    python respeaker_4mic_array.py
    ``` 

<p align="center">
<img alt="APAr" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/APA.png width=382 hight=287>

 
 
</details>



<details id=1>
<summary><h2>Step 2: Picovoice voice control</h2></summary>
 
> [Picovoice](https://picovoice.ai) is a real-time wake word detection platform for Raspberry Pi systems, running fully Automatic Speech Recognition (ASR) to perform hot word detection. Users can customize wakeup words freely and use without network connection. For keyword spotting, traditional neural networks use multi-digit numbers for calculation. Picovoice uses very short numbers, such as binary ones and zeros, so speech capture can run on chips that are much slower. Running on Raspberry PI consumes less than 10% of the CPU.
 
1. Set up

    :point_down: Install pyaudio driver and Picovoice demo for Respeaker
&nbsp;
 
    ```
    pip3 install pyaudio
    pip3 install pvrespeakerdemo
    ``` 


    :runner: Run the Picovoice demo 
&nbsp;
 
    ```
    picovoice_respeaker_demo
    ``` 
 

    > The program will open in the terminal and speak the keyword "Picovoice" into the microphone, the system will capture the keyword, wait for the user to say the command and complete the command.

    :speaking_head: Try to say the keyword “Picovoice” and the command “turn on the lights”.

 
<p align="center">
<img alt="Terminal1" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/terminal1.png width=544 hight=322>

 
<p align="center">
<img alt="Terminal2" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/terminal2.png width=544 hight=125>
 
<p align="center">
<img alt="LEDblue" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi.io/blob/main/img/LEDblue.png width=382 hight=287>

 
2. Auto start

    > Set Pi to automatically open the command line after startup, and automatically execute the program in it

    :open_file_folder: Create a new folder to autostart
&nbsp;
 
    ```
    cd /home/pi/.config 
    mkdir autostart
    cd autostart
    ``` 

 
    :computer: Create a "pico.desktop" in "autostart" folder and type the text follow
&nbsp;
 
    ```
    [Desktop Entry]
    Name=PChost
    Comment=Python Program
    Exec=lxterminal -e picovoice_respeaker_demo
    Icon=/home/pi/python_games/picovoice.png
    Terminal=false
    MultipleArgs=false
    Type=Application
    Categories=Application;Development;
    StartupNotify=true
    ``` 

3. Voice commands

    :speaking_head: Wake word
&nbsp;
 
    ```
    Picovoice
    ``` 

    :speaking_head: Ture on/off the light
&nbsp;
 
    ```
    [switch, turn] [on, off] (all) (the) [lights, light]
    [switch, turn] (all) (the) [light, lights] [on, off]
    ``` 

    :speaking_head: Change the color
&nbsp;
 
    ```
    [change, set, switch] (all) (the) (light, lights) (color) (to) [blue, green, orange, pink, purple, red, white, yellow]
    ``` 
</details>


<details id=1>
<summary><h2>Step 3: Sound source localization and tracking</h2></summary>

> For sound source localization and tracking, the main artificial intelligence framework used in this project is the new [Open embedded Audition System](https://github.com/introlab/odas). The working principle of this framework is compressed space into a unit sphere, the sound card is taken as the center of the sphere and the sound source position of the surrounding environment is detected with a radius of one meter. This framework uses microphone array geometry to perform sound source localization. This method calculates the Steered-Response Power with Phase Transform (SRP-PHAT) by using the difference between the arriving time, which is obtained by using the sum of GCC- PHAT for each pair of microphones (TDOA)-related cross-correlation values. For more imformation, check the article "[ODAS: Open embeddeD Audition System](https://arxiv.org/pdf/2103.03954.pdf)". 
 
1. For ODAS Client:
&nbsp;
 
    ```
    sudo apt-get install libfftw3-dev libconfig-dev libasound2-dev libgconf-2-4 sudo apt-get install cmake
    git clone https://github.com/introlab/odas.git
    mkdir odas/build
    cd odas/build
    cmake ..
    make
    ``` 

2. For ODAS Server:
&nbsp;

    :point_down: Install Node.js v12
&nbsp;
 
    ```
    curl -sL https://deb.nodesource.com/setup_12.x | 
    sudo bash - sudo apy-get install -y nodejs
    ``` 

    :scissors: Clone the repository
&nbsp;
 
    ```
    git clone https://github.com/introlab/odas_web.git 
    cd odas_web/
    npm install
    ``` 

3. Start ODAS studio


    :keyboard: Tap in terminal
&nbsp;
 
    ```
    npm start
    ``` 

    > In addition to the sound source, the detector can also observe the Raspberry Pi's real- time performance (CPU usage, CPU temperature, memory usage, etc.). Filter function can be set to control the accuracy of source location and tracking. Source Elevation refers to the elevation of the sound source, and Source Azimut refers to the position of the source around the Z axis relative to the X axis. Active Sources Locations allows direct observation of sphere and sound card in real time.
 
<p align="center">
<img alt="ODAS" src=https://github.com/WLi0777/Hearing-aids-research-under-Raspberry-Pi/blob/main/img/ODAS.PNG width=606 hight=404>

  

</details>



<details id=1>
<summary><h2>Step 4: Alexa voice assistant</h2></summary>





</details>





<details id=1>
<summary><h2>Step 5: Hearing aids basic functions</h2></summary>





</details>


<details id=1>
<summary><h2>Step 6: SSH for remote access</h2></summary>





</details>






