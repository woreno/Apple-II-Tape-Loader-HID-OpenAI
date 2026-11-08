# Apple-II-Tape-Loader-HID-OpenAI
This is an Apple II mod that allows talking with your Apple II with Artificial Inteligence (doing commands, loading tapes, writing Basic code); loading tapes from a SDCard and have USB keyboard and joystick emulation. 
It was done in second half of 2023, i wanted to test OpenAI API so i need a good project, so its still stuck to old OpenAI API formats. 
This project implies some DIY modification to the Apple II board for switching purposes. I think is too complicated for accomplish such a simple task, i wished i didn't have to use a PC with a server application, instead just using just the Raspberry Pi Pico - i think having a speak synthesis library and space for conversions is asking too much for Raspberry Pi Pico.

**This mod consists of**
  * amplifier op-amp circuit with a electrec microphone wired to the Raspberry Pi Pico, also a button to start/stop recording
  * an ESP8266 to connect to a PC with a server application that will communicate with OpenAI
  * a circuit that selects which line drives the speaker (either Raspberry Pi Pico or I/O), which line drives the tape audio for loading to the I/O ((either Raspbery Pi Pico or cassete input)
  * a circuit that shift bits to 2 latches and puts the factory keyboard and paddles behind a buffers. The first latch/buffer one allows to the "Apple II keyboard" or the "Raspberry PI Pico generated Keyboard or USB Keyboard" to be active at a time; The second latch/buffer allows the "paddles" or the "USB gamepad" to be active at a time - i didn't finish the code for the USB gamepad as didn't had one to test it
  * a mini TFT ST7755, joystick for user interface and a SDCard reader with a SDCard with Apple II 1-bit-digitalized tapes and a root file "CATALOG.TXT" with the full-name of each tape; this allow to load tapes directly from this small TFT

**Heavy modifications**
 * to the Raspberry Pi Pico be able to load tapes, and emmit sound its required to CUT at the speaker and the buffered cassete input and have a circuit with a 74LS257 for multiplex the pins.
 * to be able to put a buffer behind the digitalized outputs (X-Y axis pulses, and switches) from the game-paddles is needed to cut these and put it, behind a buffer.
 * these 2 are heavy modifications for an original Apple II, as warned before, i built before an Apple II board so i was more confortable with that idea of modding it.

**Description**
  * USB Keyboard: when pressed a key on the USB keyboard the Pico changes the keyboard interface and sends keystrokes from the USB Keyboard to the Apple II; after a small period of inactivity the control is transfered to the Apple II original keyboard. Since USB keyboard keystrokes are intercepted so special commands are possible when Apple II is running its command-line. 
  * Commands: From the command-line with the USB Keyboard is possible the special commands: AI (ask a question), LOAD, SAVE, CAT (list tapes on SDCard), RES (reset), MEM (memory check). These commands also can be invoked by natural speech by pressing the microphone button. Its also possible to download games directly from the TFT screen (this will use the Apple II 2 server games https://asciiexpress.net/gameserver/)
  * OpenAI: at the beggining the Raspberry Pi Pico transfer the CATALOG.txt containing the filename and the full-name of each tape. It will build an initial Context Window this tape list and instructions how to process questions, endorsing Apple II code convections, format information and code chunks delimited with tags, and how to process commands into command tags.
  * Microphone operation: When the microphone button is pressed the sound is recorded (if there's a silence threshold automatically will stop); the sound is uploaded to the server and transfered to OpenAI. Next the server will read the response formated with tags, and it will transfer it to Raspberry Pi Pico in different packets. If part of a response isn't a code or command, apart of sending a first packet with text, it will convert that text into voice using iSpeak library then lowering the resolution to be suitable for PWM output by the Raspberry Pi Pico. Once the packets arrive at Pico, the text is written in screen (sending keystrokes) and the voice is emitted by PWM to the Apple II speaker. So its possible to ask a general question and seeing the response being written on screen along voice, or as for example, to tell "Load Mario Brothers", "List all tapes", "Do memory checks", "Clear", or "Write a basic code for a Mandelbrot fractal".
    






