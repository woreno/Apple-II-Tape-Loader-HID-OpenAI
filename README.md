# Apple-II-Tape-Loader-HID-OpenAI (still cleaning the project to Github)
This is a Apple II mod that allows talking with your Apple II with Artificial Inteligence (doing commands, loading tapes, writing Basic code); loading tapes from a SDCard and have USB keyboard and joystick emulation. 
It was done in mid-2023, and my first OpenAI project, so still stuck to old OpenAI API formats.

**This mod consists of**
  * amplifier op-amp circuit with a electrec microphone wired to the Raspberry Pi Pico
  * an ESP8266 to connect to a server on a PC that will communicate with OpenAI
  * a circuit that connect/disconnects the ear-jack line from the Apple II enabling Raspberry Pi Pico to deliver sound (for loading tapes)
  * a circuit that intercepts the original Apple II Keyboard and can replace it by a Raspberry Pi Pico generated keyboard (for writing on screen) or a USB Keyboard 
  * a mini TFT ST7755, joystick for user interface and a SDCard reader with a SDCard with Apple II 1-bit-digitalized tapes and a root file "CATALOG.TXT" with the full-name of each tape

**Description**
  * USB Keyboard: when pressed a key on the USB keyboard the Pico changes the keyboard interface and sends keystrokes from the
    USB Keyboard to the Apple II; after a small period of inactivity the control is transfered to the Apple II original keyboard.
  * When Apple II is running its command-line keystrokes are intercepted by the Raspberry Pi Pico - if they are special commands like "ASK" they will call the server, the server will communicate with OpenAI and reply accordantly.
    * Other special commands are possible like LOADTAPE, or LISTTAPES they will load or print the tapes available on SDCard. 
  * OpenAI: when a button is pressed the sound is recorded using the microphone connected to the Raspberry Pi Pico; the sound is uploaded to the server and this will communicate to OpenAI. There is a context window that indicates the LLM how to format a response in chunks delimited with special tags. The chunks that are information, the server will text-to-speak and lower the sound resolution and transfer these voices along the text chunks. At the Apple II side, the Raspberry Pi Pico receiving the response chunks, will PWM the voices that are informational, while other chunks like Apple II BASIC code will just print on screen. This results in Apple II speaking the words as writting the text. The special commands are also understood - the Raspberry Pi Pico will send the voice commands to the server, this one will transfer the sound to OpenAI and in return, a tag with the special command comes back. At the startup, the list of tapes is uploaded to the server, and the context window is initialized with these (filename and longname). So its possible to ask, in example, to "Load Mario Brothers", "List all tapes", "Do memory checks", "Clear", or "Write a basic code for a Mandelbrot fractal" or asking a normal question, that "Apple II" will write on screen the response while speaking it.
    






