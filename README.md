# LAB 1 

1. **Step-by-Step Setup Instructions**  
   - Each setup step is extensively explained, including the purpose of every term, directory, and command used.
2. **Lab Questions (Theory Only)**  
   - Since there will be no practical test in the lab, this section focuses on theoretical questions that might appear in a lab exam, with detailed answers explaining the rationale behind each step.

---

## Understanding the Basics

Before diving into the setup, let’s clarify common terms and concepts that you will encounter:

- **Raspberry Pi 400**: A compact device that integrates a Raspberry Pi 4 model into a keyboard. It runs on a Broadcom System on a Chip (SoC) and uses a **microSD card** to store its operating system and files.
- **microSD Card**: A small, removable flash memory card used to store the Raspberry Pi’s operating system (OS) and other data. 
- **Operating System (OS)**: The software that manages all of the hardware (like USB ports, Wi-Fi chips, etc.) and provides an environment where applications can run. We are using **Raspberry Pi OS** (a version of Linux).
- **Logitech C310 HD Webcam**: A USB webcam that provides 720p (HD) video and has a built-in microphone.
- **USB**: Stands for Universal Serial Bus, a common connector type for devices such as webcams, keyboards, and mice.
- **SSH (Secure Shell)**: A cryptographic network protocol that allows you to connect securely to the Raspberry Pi’s command line (terminal) from another computer on the same network.
- **VNC (Virtual Network Computing)**: A graphical desktop-sharing system that lets you remotely access the Raspberry Pi’s desktop environment as though you have a monitor, keyboard, and mouse connected directly.
- **Hostname**: A name used to identify a device on a network (e.g., `raspberrypi.local`).
- **Command Line / Terminal**: A text-based interface to interact with the operating system using commands.
- **Directory**: A folder in a file system. For example, `/etc/` is a standard Linux directory that holds many configuration files.
- **`sudo`**: A command in Linux that stands for “superuser do.” It temporarily grants administrator (root) privileges to run commands that typically require higher-level access.
- **`apt`**: Stands for Advanced Package Tool. It is the command-line package manager for Debian-based Linux distributions, like Raspberry Pi OS. Examples of usage include `apt install`, `apt update`, `apt upgrade`.
- **`raspi-config`**: A tool created by the Raspberry Pi Foundation to configure common options (like enabling SSH, VNC, camera interfaces, etc.) through a menu-driven interface in the terminal.
- **`dhcpcd.conf`**: A configuration file for the DHCP client daemon, which controls how network interfaces get their IP addresses. On the Raspberry Pi OS, it lives in the `/etc/` directory (`/etc/dhcpcd.conf`).
- **`fswebcam`**: A lightweight command-line tool to capture still images from a webcam.
- **`ffmpeg`**: A powerful command-line tool used to record, convert, and stream audio and video.
- **`arecord`/`aplay`**: Command-line utilities for recording and playing audio via ALSA (Advanced Linux Sound Architecture).

These explanations will be repeated or reinforced as we go through each step below.

---

## Part 1: Step-by-Step Setup Instructions

### Materials Needed

- **Raspberry Pi 400**  
- **MicroSD card** (with at least 16 GB recommended)  
- **Logitech C310 HD Webcam**  
- **Internet Connection** (Mobile hotspot or home router)  
- (Optional) **Laptop or Desktop Computer** to prepare the microSD card and to remotely connect to your Pi

---

### Section 1: Setting Up Raspberry Pi 400

#### 1. Install Raspberry Pi OS onto the MicroSD Card

1. **Download the Official Imager Software**  
   - Go to [Raspberry Pi website](https://www.raspberrypi.com/software/) or directly to [imager_latest.exe](https://downloads.raspberrypi.org/imager/imager_latest.exe) (if you are on Windows). This software is officially provided by the Raspberry Pi Foundation.
   - **Why**: The Raspberry Pi Imager writes the Raspberry Pi OS image onto the microSD card in the correct format.

2. **Connect the MicroSD Card to Your PC**  
   - Insert the microSD card using a card reader or your computer’s built-in SD card slot.
   - **Why**: This allows your computer to read and write data to the microSD card.

3. **Start the Imager Software**  
   - Launch “Raspberry Pi Imager.”
   - **Choose OS**: Select “Raspberry Pi OS” (32-bit) or another suitable version.  
   - **Choose Storage**: Select your microSD card from the list.

4. **Click the Settings Gear Icon (⚙️) [Edit Settings]**  
   - **Hostname**: Enter a unique name (e.g., `raspberrypi`).
   - **Enable SSH**: Check this option. This is crucial if you want to remotely connect from your laptop’s terminal.
   - **Set Username and Password**: You can choose “pi” as the username for convention, or create a new one.
   - **Configure WiFi**: Provide your wireless SSID (WiFi name) and password.  
   - **Why**: These configurations streamline the process so you don’t need to manually set them up after first boot.

5. **Write the OS to the microSD Card**  
   - Click **Write**.  
   - A warning may appear; confirm to proceed. This will erase any existing data on the microSD card.
   - Once writing is done, safely eject the microSD card from your computer.

---

### Section 2: Configuring Raspberry Pi

#### 1. Assembling Hardware

1. **Insert the MicroSD Card**  
   - Gently push the microSD card into the slot on the Pi 400.  
   - **Directory Relevance**: The Pi 400 will look for its boot files on the `/boot/` directory located on this microSD card.

2. **Connect the Power Adapter (USB-C)**  
   - Use a Raspberry Pi 400-compatible power supply.  
   - **Why**: The Pi 400 requires about 5V, 3A of power to function reliably.

3. **Plug in the Webcam**  
   - Insert the Logitech C310 webcam into a USB 2.0 or 3.0 port on the Pi 400.
   - **Directory Relevance**: The `/dev/` directory in Linux contains hardware device files like `/dev/video0` for your webcam.

4. **Power On the Pi**  
   - As soon as the power is connected, the Pi 400 will begin to boot from the microSD card.

#### 2. Enabling VNC (Virtual Network Computing)

1. **Find the Raspberry Pi’s IP Address**  
   - On your router/mobile hotspot settings page, look for connected devices. The Pi will appear by the hostname you set.  
   - Alternatively, type `raspberrypi.local` in an SSH client.  
   - **Directory Relevance**: The Pi’s network configurations are stored in files under `/etc/`, such as `/etc/dhcpcd.conf`.

2. **SSH into the Pi**  
   - Use `PuTTY` on Windows or the Terminal in macOS/Linux:  
     ```bash
     ssh pi@<ip_or_hostname>
     ```
   - **Why**: SSH (Secure Shell) allows command-line access over a network. This is essential if you’re not directly connected to the Pi with a monitor.

3. **[Optional] Install haveged**  
   ```bash
   sudo apt install haveged
Terms: sudo = “superuser do,” grants admin privileges. apt install = “install a package using the Advanced Package Tool.”

Why: On some Raspberry Pi systems, haveged improves the generation of random numbers (entropy), which can be required for secure connections like VNC.

Enable VNC Using raspi-config

bash
Copy
Edit
sudo raspi-config
Choose Interfacing Options → VNC → Yes.

Directory Relevance: raspi-config modifies configuration files in /boot/config.txt and also in system directories like /etc/rc.local or service definitions to enable VNC.

3. Setting Up a Static IP (Optional)
Edit the dhcpcd.conf File

bash
Copy
Edit
sudo nano /etc/dhcpcd.conf
Why: nano is a text editor. /etc/dhcpcd.conf is the main configuration file for the DHCP client daemon that manages IP addresses.

Add Configuration Lines (Example):

bash
Copy
Edit
interface eth0
static ip_address=192.168.1.50/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
interface eth0: Tells the system we are configuring the Ethernet interface.

static ip_address=192.168.1.50/24: Sets a static address of 192.168.1.50 with a subnet mask of /24 (255.255.255.0).

static routers=192.168.1.1: IP of your network’s router.

static domain_name_servers=192.168.1.1: DNS server address, often the same as the router.

Save & Exit

Press CTRL+X, then Y, then Enter to confirm in nano.

Reboot

bash
Copy
Edit
sudo reboot
The Pi will restart, and the static IP will be assigned at boot.

4. Installing VNC Viewer on Your Laptop
Go to the RealVNC Website

Download VNC Viewer for your operating system.

Why: You need a client application to remotely view the Pi’s desktop.

Install & Launch

Installation steps will vary per OS.

Connect to the Pi

Enter your Pi’s IP address (or hostname).

Provide the username and password when prompted.

Section 3: Setting Up and Testing the Webcam
1. Connecting the Webcam
After plugging it into the Pi’s USB port, you can check if the system detects it using:

bash
Copy
Edit
lsusb
lsusb lists all USB devices. Expect to see something like “Logitech, Inc. HD Webcam C310.”

2. Installing Software to Capture Images: fswebcam
bash
Copy
Edit
sudo apt update
sudo apt install fswebcam
apt update: Updates the package list.

apt install fswebcam: Installs the fswebcam package.

3. Capturing an Image
bash
Copy
Edit
fswebcam -r 1280x720 --no-banner image.jpg
-r 1280x720: Resolution for the captured image.

--no-banner: Disables timestamp and other text overlays in the final image.

image.jpg: The file name that the picture will be saved as in the current directory.

4. Viewing & Editing Images (Optional)
bash
Copy
Edit
sudo apt install gimp
GIMP is a full-featured image editor. You can open image.jpg with GIMP if you want to crop or annotate it.

5. Recording Video
Install ffmpeg

bash
Copy
Edit
sudo apt install ffmpeg
ffmpeg is a versatile tool for audio/video operations.

Record

bash
Copy
Edit
ffmpeg -f v4l2 -framerate 25 -video_size 640x480 -i /dev/video0 output.mp4
-f v4l2: Using Video4Linux2 drivers.

-framerate 25: Capture 25 frames per second.

-video_size 640x480: Resolution of the video.

-i /dev/video0: Input device file for the first webcam.

output.mp4: The output filename.

6. Playing Video (Optional)
bash
Copy
Edit
sudo apt install vlc
vlc output.mp4
VLC is a popular media player that can handle MP4 and many other formats.

7. Checking for Audio Device
bash
Copy
Edit
arecord -l
arecord -l: Lists recording devices recognized by ALSA.

Look for the webcam’s microphone, typically labeled as “Card X, Device Y.”

8. Recording Audio with arecord
bash
Copy
Edit
arecord -D plughw:2,0 -d 10 test.wav
-D plughw:2,0: Tells arecord to use card 2, device 0.

-d 10: Record for 10 seconds.

test.wav: Output file name.

9. Playing the Recorded Audio
bash
Copy
Edit
aplay test.wav
aplay: A playback utility for ALSA.

Section 4: Advanced Applications (Optional)
a. Setting up and Using a Python Virtual Environment
Install python3-venv

bash
Copy
Edit
sudo apt install python3-venv
Why: You need this to create and manage isolated Python environments.

Create a Virtual Environment

bash
Copy
Edit
python3 -m venv myenv
This creates a folder named myenv containing the isolated Python installation.

Activate the Environment

bash
Copy
Edit
source myenv/bin/activate
Your shell prompt will change, indicating you are now “inside” this virtual environment.

Install Packages

bash
Copy
Edit
pip install opencv-python
Why: Installing opencv-python within the virtual environment prevents conflicts with system-wide packages.

Deactivate

bash
Copy
Edit
deactivate
Returns you to the global environment.

b. Python Scripting for the Webcam
Below is an example script that captures a single frame from the webcam:

python
Copy
Edit
import cv2

cap = cv2.VideoCapture(0)  # 0 means the first webcam device (usually /dev/video0)
if not cap.isOpened():
    print("Cannot open camera")
    exit()

ret, frame = cap.read()  # ret is a boolean indicating success, frame is the captured image
if not ret:
    print("Can't receive frame. Exiting...")
    exit()

cv2.imwrite("captured_frame.jpg", frame)
cap.release()
cv2.destroyAllWindows()
cv2.VideoCapture(0): Initializes the webcam.

cap.read(): Reads a frame from the webcam.

cv2.imwrite(...): Saves the frame to a file.

c. Mock Exercise - Creating a Surveillance System
Motion Detection using frame differencing (comparing consecutive frames).

Alerting or drawing bounding boxes on movements.

Potential to automate alerts, store logs, or record when motion is detected.

Part 2: Lab Questions (Theory Only)
Since there is no practical test, here are possible theoretical lab questions you may face, along with detailed rationales and definitions:

1. How do you enable SSH and VNC on a fresh Raspberry Pi OS installation?
Answer:

SSH can be enabled in two ways:

Via the Raspberry Pi Imager’s advanced settings before flashing.

Using sudo raspi-config → Interfacing Options → SSH → enable.

VNC is enabled similarly through raspi-config.

Rationale: raspi-config modifies underlying configuration files that control whether these services are active at startup.

2. Explain the purpose of /etc/dhcpcd.conf and what happens if you modify it.
Answer:

/etc/ indicates it’s a configuration directory in Linux.

dhcpcd.conf is the config file for the DHCP client daemon. By adding lines like static ip_address=, you instruct the Pi to request or force a static IP.

Rationale: This is how the OS knows to bind a specific IP address to a network interface (like eth0).

3. What is the difference between fswebcam and ffmpeg in terms of functionality?
Answer:

fswebcam is a lightweight tool primarily for still images.

ffmpeg is more robust and can handle video recording, audio capture, conversions, and streaming.

Rationale: Understanding tool capabilities helps choose the right one for a particular job.

4. Why do we use sudo for certain commands?
Answer:

sudo means “superuser do.” Certain system files/directories (like /etc/) require administrative privileges to modify.

Rationale: This ensures that only authorized users can change critical system settings.

5. Explain the steps to record audio from the webcam microphone and then play it back.
Answer:

Identify device via arecord -l.

Use arecord -D plughw:<CardNumber>,<DeviceNumber> -d 10 filename.wav.

Play back with aplay filename.wav.

Rationale: This demonstrates an understanding of ALSA’s record/play commands.

6. What is the rationale for using a virtual environment (e.g., python3-venv) when installing packages like OpenCV?
Answer:

Prevents conflicts with global Python packages.

Allows different projects to use different library versions.

Rationale: Good practice for Python development, ensuring system stability.

7. Describe how you could use OpenCV functions to implement a simple motion detection script.
Answer:

Read frames continuously in a loop.

Convert frames to grayscale (less data, easier to compare).

Use cv2.absdiff to compare frames (detect changes).

Apply threshold and contour detection to isolate movement.

Rationale: Illustrates basic image processing steps.

8. Why might an institutional WiFi (like SIT WiFi) block or complicate Raspberry Pi connections?
Answer:

Some networks have captive portals or advanced authentication that the Pi’s default OS settings can’t easily handle.

Administrators often block unknown MAC addresses for security.

Rationale: Explains networking constraints in certain environments.

9. Explain the role of the /dev/ directory on a Linux system.
Answer:

/dev/ is a special directory that contains device files representing hardware components (e.g., /dev/video0 for a webcam).

Rationale: Linux treats hardware as files, allowing software to read/write data as though it were a normal file.

10. What does the term ‘headless setup’ mean in the context of Raspberry Pi?
Answer:

It refers to running the Pi without a monitor, keyboard, or mouse, relying solely on remote connections like SSH or VNC.

Rationale: This is common in IoT or server scenarios, saving space and hardware requirements.

Conclusion
This README has provided a thorough guide to:

Setting up the Raspberry Pi 400, including installing Raspberry Pi OS and configuring essential services like SSH and VNC.

## Second Part
A5 (continued). ... Butterworth is a nice default choice when you want a smooth filter without worrying about ripple or phase distortions too much. In our lab, using a Butterworth band-pass gave us a clean isolation of that high-frequency band without complicating matters. In summary, a Butterworth filter is chosen for its simplicity and flat passband, making it suitable when you want an even response within your band of interest and can tolerate a moderate roll-off rate outside the band. Q6. The band-pass filter was set to 19.4 kHz - 19.6 kHz. If nothing in the environment emits frequencies in that range, what will the filtered waveform look like?
A6. If there is no significant signal in the 19.4–19.6 kHz band (which is likely, since that’s near the upper limit of human hearing and most common sounds won’t have much energy there), then the filtered waveform will be near zero. Essentially, the filter will “find nothing” to pass through except perhaps a tiny amount of background hiss. On the graph, you’d see the “FILTERED” waveform as a flat line (maybe with some small random noise) while the original waveform might show normal speech or other noises. This is expected because the filter is doing its job: removing everything except that narrow ultrasonic band, and if that band is silent, the output is silence. If you wanted to actually see the filter in action, you could generate or play a tone in that range. Otherwise, it’s a good demonstration that a filter can completely eliminate content outside its band – here it essentially zeroes out the audible parts of the signal, because those are outside 19.4–19.6 kHz. Q7. If we wanted to change the filter to isolate, say, the frequency range of human voice (300 Hz to 3400 Hz, like a telephone band), what would we need to adjust in the code?
A7. We would modify the design_filter call to use lowfreq=300 and highfreq=3400, and use fs=RATE (44100) appropriately. For example:
python
Copy
sos = design_filter(300, 3400, RATE, order=3)
This would create a band-pass filter that passes frequencies ~300 Hz to 3.4 kHz. Everything else (below 300 Hz and above 3400 Hz) would be attenuated. The rest of the code (applying sosfilt and plotting) remains the same. If we run with that, the "FILTERED" waveform would contain mainly the speech frequencies; very low rumble or high hiss would be removed. We could then, for instance, speak into the mic and the filtered waveform would look like a perhaps slightly muffled version of the original (since high treble and low bass are cut out). Essentially, we’d be simulating the effect of a telephone frequency response. Q8. What is an MFCC and why are MFCCs commonly used in speech recognition instead of using raw FFT data or spectrograms?
A8. MFCC stands for Mel-Frequency Cepstral Coefficients. They are a set of features that succinctly describe the shape of the audio spectrum on a Mel scale (which is a perceptual scale of pitch). Here’s why they’re used and what they represent:
To compute MFCCs, you take a Mel spectrogram (which already compresses frequency information into about 40 bands or so in a perceptually meaningful way), take the log of it, and then perform a Discrete Cosine Transform (DCT) across the frequency axis. The resulting coefficients (the cepstral coefficients) represent the broad shape of the spectrum (like its envelope).
The first coefficient (MFCC0) often relates to overall energy (which some systems drop), and the next few coefficients correspond to the dominant resonant characteristics of the sound (formants in speech). Because speech sounds (phonemes) are largely distinguished by the positions of formants (resonances in the vocal tract), MFCCs capture these distinctions well.
Why not raw FFT or spectrogram? Raw FFT has a lot of data (e.g., 1024 bins per frame) including a lot of redundancy and fine detail that isn’t necessary for recognizing phonemes. Direct FFT data also doesn’t account for perceptual importance (a difference at 100 Hz vs 5000 Hz might be treated equally by a machine if using raw FFT, but perceptually the 100 Hz difference might be less critical for intelligibility). A Mel spectrogram addresses perceptual scaling, and MFCC further reduces dimensionality.
MFCCs also decorrelate features. Adjacent frequency bins in an FFT are highly correlated (if one bin has energy, the next likely does too especially for broad sounds). Many machine learning models (historically Gaussian mixture models, etc.) assume features are uncorrelated; the DCT helps achieve that.
By using MFCCs, we drastically reduce the amount of data: instead of hundreds of FFT bins, we might use 13 MFCCs per frame (or 40 as in our lab for more detail). This makes the processing and classification easier and often more robust, as minor variations or noise tend to affect higher-order coefficients which we might discard.
In summary, MFCCs focus on the spectral envelope (broad shape) rather than detailed spectral lines, and that’s usually enough to identify speech sounds. They also mimic aspects of human auditory processing (Mel scale and log amplitude). That’s why virtually all traditional speech recognition systems use MFCC or similar cepstral features as input rather than raw waveforms or spectra.
Q9. During speech recognition, why do we call r.adjust_for_ambient_noise(source) before listening, and what could happen if we didn’t?
A9. The call to r.adjust_for_ambient_noise(source) listens to the environment for a short moment (by default, 1 second) to gauge the background noise level. It then sets an internal threshold in the recognizer for what is considered silence versus speech. If we didn’t call this:
The recognizer might use a default threshold. If the actual environment is noisier than the default assumption, the recognizer might think the noise is speech or might not properly detect the start/end of speech. For example, it could start recording too early (on noise) or not stop recording because it thinks the noise is part of speech.
Conversely, if the environment is much quieter than assumed, the recognizer might be too "deaf", requiring a louder input to trigger.
By adjusting for ambient noise, we basically calibrate. After that, the r.listen() will ignore anything below the new threshold as just background. This means the user doesn’t have to be in a perfectly silent room – the system adapts to, say, the hum of an AC or computer fan.
If we skip it in a noisy room, the transcription might come out as garbage or we might get an UnknownValueError more often because the noise confuses the speech-to-text. If we skip it in a silent room, it might be okay because the default threshold might suffice (quiet environment matches assumption). But as a practice, it’s good to always adjust, especially on devices like Pi which might be used in varied environments.
One thing to note: you should be quiet during that ambient noise adjustment period. If you speak or there’s a sudden loud sound during calibration, it will mis-calibrate (set threshold too high) and then your actual speech might be considered as background and not captured properly.
Q10. The speech recognition results showed that Google’s service recognized my phrase correctly, but Sphinx got it wrong or couldn’t understand. Why is there such a difference in performance? What are the trade-offs between using an online API like Google and an offline engine like Sphinx?
A10. The difference in performance comes down to the scale and sophistication of the models:
Google Speech Recognition API: This uses Google’s state-of-the-art speech-to-text model, which is likely a deep neural network trained on thousands of hours of diverse data, and knows a vast vocabulary. It also uses powerful language models to guess what words make sense together. So it’s extremely accurate for a wide range of speech (accents, noisy conditions, etc.). However, it requires an internet connection since the audio has to be sent to Google’s servers. Also, there might be usage limits or privacy considerations (your speech is sent to the cloud). The response might take a moment due to network latency.
CMU Sphinx (PocketSphinx): This is an older, lightweight engine that runs locally. It typically uses Hidden Markov Models (HMMs) with pre-trained acoustic models and a fixed dictionary. The default English model it uses is not nearly as comprehensive as Google’s. It might not know certain words or names, and it might be more easily confused by noise or different accents. It’s also usually tuned for a certain type of audio (perhaps 16kHz telephony speech). The advantage is it runs offline entirely on the Pi – no internet needed, and it can be very fast for short utterances. But the trade-off is lower accuracy, especially for arbitrary phrases.
Trade-offs:
Accuracy vs. Resources: Google’s accuracy is very high but needs cloud resources; Sphinx’s accuracy is moderate and might require you to speak clearly and maybe use short, simple phrases to get decent results, but it runs on-device.
Latency: Surprisingly, Sphinx can be faster (no network overhead) for short inputs, but if the Pi is slow or the input long, Sphinx might actually take a while to process. In our timing, Sphinx was pretty fast (maybe ~1 second) because the phrase was short. Google took ~2 seconds perhaps due to network. For longer sentences, Google might actually stream results faster than Sphinx would finish processing because Google’s infrastructure is very optimized.
Customization: Sphinx can be tuned or given a limited vocabulary/grammar to greatly improve its accuracy in specific contexts (for example, you can limit it to listen for only a set of 10 commands – then it will do much better on those). Google’s API is general and cannot be easily customized on the free tier.
Privacy: Using Sphinx keeps everything local (no audio leaves the Pi), which could be important for sensitive applications. Google’s requires sending data out, which some applications might avoid.
In summary, the Google API is better for general-purpose, high-accuracy recognition (hence it got your phrase right without fuss), while Sphinx offers offline convenience but typically needs either very clear input or some tuning to approach good accuracy. The lab demonstrates this so you see that “edge” speech recognition is possible but often inferior to cloud-based solutions, which sets up the motivation for techniques in later labs (like quantized models on edge devices, etc.).
Q11. Suppose your Raspberry Pi is not connected to the internet. How would the Google and Sphinx recognition calls behave?
A11. If there’s no internet:
The call to r.recognize_google(audio) will fail to connect to Google’s servers. In our code, this would raise a RequestError exception, which we catch and print as “Could not request results from Google...”. So you’d see that error message. No transcription would be obtained from Google.
The Sphinx recognizer, on the other hand, does not require internet. It would still run locally. So r.recognize_sphinx(audio) would proceed and attempt to transcribe the audio using the offline model. If Sphinx manages to understand, it will print the result; if not, it might print “could not understand audio” but at least it tried. Essentially, Sphinx provides a fallback when offline.
This illustrates a major advantage of having an offline recognizer: even without connectivity, your Pi can still do basic speech recognition (maybe for simple commands). If you relied solely on Google’s API, your voice feature would be non-functional without internet.
Q12. The real-time audio plotting uses a loop that reads and then processes and plots. If the processing/plotting took too long in each iteration, what would happen? How could we ensure we don’t miss any audio data?
A12. If processing each chunk takes longer than the time it takes for that chunk of audio to be recorded, we’ll start to fall behind. In our example, each chunk is ~0.37 seconds of audio (16384 samples at 44100 Hz). If our loop iteration took, say, 0.5 seconds to execute (because maybe we chose a super large FFT or did something heavy), then while we’re still processing, new audio is coming in but we’re not reading it. The audio buffer in the input stream could overflow (leading to dropped samples or a backlog).
In practice, PyAudio might have an internal buffer that can hold some number of frames, but if you don’t call stream.read() fast enough, eventually it will overflow and you’ll lose data or get a delay. You might hear glitches or see jumps in the data.
This is a real-time processing constraint: the processing must keep up with the data rate.
To ensure we don’t miss data, we have a few options:
Use a smaller buffer so that each chunk is shorter in duration, giving less work per chunk (but more frequent chunks). This can reduce per-iteration load, but the overall processing per second remains similar – it just slices it finer, which sometimes helps with keeping the UI responsive.
Optimize the processing – e.g., using NumPy and avoiding Python loops (which we did), perhaps using smaller FFT size if resolution can be compromised, or not plotting every single frame (plotting can be slow if too frequent).
Use a separate thread or callback for audio – PyAudio supports a callback mode where audio is fetched in the background. You could have audio put into a queue, and your main loop pull from that queue to process. This way the audio grabbing isn’t delayed by processing. It adds complexity (thread synchronization, etc.), but it’s how professional audio software ensures continuous capture.
Lower the sample rate – if 44.1 kHz is overkill for the application, using 16 kHz halves the data to process, for example.
In our lab’s case, the processing was fast (FFT took only a few milliseconds) and plotting at ~3 frames/sec is fine, so we didn’t miss data. But it’s an important consideration in real-time systems: your algorithm’s throughput must be >= data throughput, or you implement buffering and multi-threading to handle occasional slowness.
Q13. The chroma feature we plotted shows 12 bands (C, C#, D, ..., B). What could such a plot tell us about an audio clip? Provide an example scenario.
A13. A chromagram indicates which musical notes (ignoring octave) are present at each moment. For example, consider an audio clip of someone playing a C major chord on a piano (notes C, E, G across possibly several octaves). In the chroma plot, during that chord, you’d see the bands for C, E, and G light up strongly, because the frequencies corresponding to any C, E, and G (in any octave) will all be summed into those chroma bins. If the next chord was F major (F, A, C), you’d see F, A, C bands light up.
In vocals or speech, chroma is less directly meaningful because speech isn’t usually harmonic in a musical sense (except when singing). However, you might still see the imprint of the fundamental frequency (pitch of the voice) and its harmonics falling into certain chroma bins especially if someone speaks in a somewhat tonal language or sings.
Another scenario: say the audio was a scale being played: C, D, E, F, G, A, B, C. The chroma plot would show one band lighting up at a time (first C, then D, then E, etc., then back to C).
So, the chroma feature is very useful in music information retrieval: it can help with chord recognition, key detection (a particular key will have certain chroma patterns over time), or aligning cover songs that are in different keys (since shifting key just rotates the chroma vector, because the relative pattern of notes changes cyclically).
For our lab, it was more to show an example of an advanced feature extraction. It demonstrates how you can reduce a complex spectrum into something as simple as “which notes are present now”. Even if one doesn’t fully understand music theory, they can see that chroma condenses the info to 12 dimensions, which is neat.
Q14. How does increasing the order of a Butterworth filter affect its frequency response and the filtered output in time domain?
A14. Increasing the order of a Butterworth filter makes the filter’s frequency response transition sharper (steeper roll-off). For a band-pass, a higher order means the edges of the band (the cutoff frequencies) will be more tightly “enforced”, attenuating frequencies outside the band more strongly. However, there are a few side effects:
The filter in time domain will have a longer impulse response (it rings more) because a sharper frequency cutoff in frequency domain implies a longer convolution kernel in time (that’s a Fourier transform pair property).
This can introduce more phase distortion (though Butterworth has non-linear phase, it’s usually not severe, but higher order can mean more phase shift around the cutoff frequencies).
In our context, if we increased order from 3 to, say, 6 (which actually would be 12th order effective for bandpass), the filtered output would potentially have slightly more delay (phase shift) and if a frequency outside the passband tried to pass, it would be even more suppressed. On the plot, the "FILTERED" waveform might look even cleaner (less leakage of unwanted frequencies).
However, a too high order filter might also be more computationally heavy (though on modern hardware up to certain order it’s fine) and more sensitive to numerical issues (hence using sos is important).
For example, if we filtered a broad-spectrum sound with an order-3 bandpass, some frequencies just outside our band might still sneak through a bit (at reduced level). With order-6, those would be practically zero. The trade-off is the filter might overshoot or ring a bit in time domain when there’s a sudden change (like an impulse) because high-order filters can have a longer settling time.
In many practical cases, order 3 or 4 is enough, but if you needed a really steep cutoff (say to isolate a tone in presence of very close other tone), you’d increase order.
Q15. What are some potential improvements or alternative approaches for the audio analytics tasks demonstrated in Lab 2?
A15. There are many ways one could extend or improve the lab's code:
Use Callback for Audio Input: As mentioned, instead of blocking reads, use PyAudio or sounddevice in callback mode. This way audio capture runs on a separate thread and calls our function whenever a buffer is ready. We can then update the plot or do analysis asynchronously. This would make the UI more responsive and ensure no data loss. It's a bit more complex to code though.
Real-time Spectrogram Display: Our real-time display just showed the current chunk’s spectrum. We could accumulate a scrolling spectrogram (like a waterfall display) by storing past FFT results in an image and updating it. This is heavier on plotting but provides a more informative view over time.
Different Audio Features: We showed chroma, MFCC, etc., for a static file. One could compute these in real-time too (e.g., update a display of MFCC values over time, or print out if the audio pitch (chroma) corresponds to a certain note being hummed). Or simpler features: track the spectral centroid (the “center of mass” of the spectrum) which often corresponds to how “bright” or “dark” a sound is; or zero-crossing rate as an indicator of noisiness.
Noise Reduction Filter: We did a simple band-pass. If the environment has a consistent noise (like 50/60 Hz hum), one could implement a notch filter to cut that out. Or use a low-pass filter to isolate low-frequency events (like footsteps).
Better Speech Recognition Offline: Instead of Sphinx, which is dated, one could use a modern offline recognizer like Vosk or DeepSpeech or Coqui STT on the Pi. These use neural networks and can be more accurate than Sphinx while still offline (though they require more CPU or even using a Pi 4's Neon or a Coral AI Accelerator).
Wake Word Detection: Continuously running recognition is expensive and error-prone. A common practice is to use a lightweight wake-word detector (like Snowboy or Porcupine engines, or even a simple MFCC+classifier) to detect a specific word (e.g., "Pi, listen"). Only then trigger the heavy recognition or cloud API. This saves resources and avoids capturing irrelevant speech.
Volume Level Indicator: We could easily compute the RMS (root-mean-square) of each audio chunk to get a volume level and display that (like a VU meter). This is simpler than an FFT and useful (like monitoring sound levels).
Use Microphone Array or Directional Mic: For better recognition in noise, hardware improvements like using a microphone array (and beamforming to focus on sound from a direction) could be mentioned, though that's beyond coding – it's a setup improvement.
Alternate Visualization: If running with a GUI is not convenient, one could print a simple ASCII spectrum in the terminal (like a text-based bar graph) for fun, or use an LED bar display connected to Pi's GPIO to create a hardware audio meter.
Time-frequency trade-off experimentation: Encourage trying different BUFFER sizes or FFT lengths to see how the spectrum resolution changes. For instance, using BUFFER of 4096 vs 16384 – the spectrum becomes less detailed in frequency (bins 10 Hz apart vs ~2.7 Hz apart) but updates more often. Students can find a sweet spot or understand the compromise.
Using Convolutional Neural Networks on Spectrogram: Pushing further (towards Labs 4-5 topics), one could capture a spectrogram and feed it into a small CNN on the Pi to do things like audio classification (identify a clap vs speech vs music). This combines the feature extraction and AI model deployment aspects.
These improvements either make the system more robust (threaded audio), more functional (additional features or recognition capabilities), or more insightful (better visualization or analysis). Depending on the project goals, one or more of these could be implemented on a Raspberry Pi given enough time and resources.

Explaining all Terms such as sudo, apt, /etc/dhcpcd.conf, and directories like /dev/.

Lab-Theory Questions that focus on the rationale behind each configuration step, ensuring you understand not just how but why each command is used.

By studying these steps and questions in detail, you should be well-prepared for any theory-based lab exam that covers Raspberry Pi setup, network configuration, and basic webcam/audio functionality.
