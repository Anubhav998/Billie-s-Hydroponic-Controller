Billie-s-Hydroponic-Controller
==============================

Here you will find the latest version of the code...

A nifty organization called Seeds of Change here in Anchorage, Alaska has been helping young people get started in productive commerce. It operates a large vertical hydroponics grow system in a converted warehouse and offers employment to learn the business of plant care. They were interested in a IOT system to help automate their water control. This instructable is mainly to document my volunteer efforts to build an affordable and expandable microcontroller system to assist in their efforts.

Large Hydroponic grow operations have come and gone over the last several years. The consolidation in this business has been marked by the difficulty in making it profitable. You have to automate like crazy by all accounts to make fancy bags of lettuce sell for a profit. These vertical units do not produce anything with any real calories--you are basically growing nicely packaged water--so you have to sell it at a premium. This water-resistant adjustable unit is built to control the water level in the main reservoir and constantly measure its depth, ph, temperature. The main unit runs on a ESP32 Featherwing and reports its findings through the web to a blynk app on your phone for monitoring and email or text warnings if things go wanky on you.

Add TipAsk QuestionCommentDownload
Step 1: Gather Your Materials
Gather Your Materials
Gather Your Materials
Gather Your Materials
The design was based on cheap water-resistant electrical boxes from Lowes and a few holders that were 3D printed. The rest of the parts are all relatively cheap except for the pH unit from DF Robot and the ETape from Adafruit. DF Robot sells their new 3 volt version of their analog pH sensor with a cheaper pH probe and you will probably have to invest in a expensive version of this one for constant immersion. I did not include a conductivity tester yet but this will probably be in an upgrade after seeing how this one fares.

1. Two gang water-resistant electrical boxes from Lowes--with various fittings to hold straight and bent tubes-$10

2. 12" Standard eTape Liquid Level Sensor with Plastic Casing Adafruit --$59 you can get this without the plastic housing for $20 less...

3. Adafruit HUZZAH32 – ESP32 Feather Board--great board.$20

4. Aiskaer 2 Pieces Side mounted Aquarium Tank Side mounted Horizontal Liquid Float Switch Water Level $4

5. Adafruit Non-Latching Mini Relay FeatherWing

6. Lipo--battery $5 (power back up)

7. Couple LED's various colors

8. Waterproof DS18B20 Digital temperature sensor + extras $10 Adafruit

9. Gravity: Analog pH Sensor/Meter Kit V2 DF Robot $39--Industrial pH probe will cost $49 more

10 Waterproof Rugged Metal On/Off Switch with Red LED Ring - 16mm Red On/Off $5

11 Plastic Water Solenoid Valve - 12V - 3/4" (Don't get the 1/2 inch--it doesn't fit anything...)

12. Diymall 0.96" Inch Yellow Blue I2c IIC Serial Oled LCD LED Module $5

Add TipAsk QuestionCommentDownload
Step 2: Wire It
Wire It
Wire It
Wire It3 More Images
Just follow the Fritzing diagram for the wiring. The esp32 was mounted on a photo board with the OLED screen on the opposite side where it would face the small hole in the central back of the gang-box. The LED's were connected to two digital outputs of the ESP. One is indicative of a WiFi connection and the other announces if the Relay is turned on to the water output. The Lipo battery is attached to the battery input on the board. All other boards (pH, relay, Etape, one-wire temp, OLED) are all powered from the 3 volts on the board. The on/off is connected to ground by the enable pin on the main board--the LED is powered by NO connection to power. The eTape is definitely something to examine carefully -- on my board the power and ground were reversed (RED/BLACK) and this seems to be the case with others who have had this problem (do search on adafruits web site for this problem...) also the resistor included in the head should be measured carefully--it is not as published. The new DH Robot board works with 3V now and so works with the ESP32. Could not get A0 to work -- doesn't take inputs before Wifi connection so I used other analog inputs.

Add TipAsk QuestionCommentDownload
Step 3: Build It
Build It
Build It
Build It3 More Images
Everything fits rather neatly into the main box. Two poles of electrical conduit fit nicely out of the waterproof nipples at the bottom. These support the measuring instruments. They can be made arbitrarily longer or shorter to suspend the box higher or lower to the water level--your only limits are the length of your connecting wires that have to go into the box. These tubes should be sealed on the bottom with silicon. The instruments are suspended from 3D printed connectors that correspond to the curvature of the etape body and the conduit. They are easily adjustable with wing nuts. Special holders for the pH probe and the One-wire temp probe were also printed. The box support for the level - water- control switches was also 3D printed. These switches are waterproof and well designed and cheap. They appear to be enclosed reed switches. The box was filled with silicon after they were secured with included nut on the inside. The distance between these switches will determine how much fluid is allowed in before shutoff. All wires are lead through a lower opening and then sealed with silicon. The pH probe wire was fed in through the upper opening as it will most likely be changed out frequently. The on/off switch was hot glued into position. A rack to securely mount the esp32 with screen was 3D printed. A tiny round plastic window was siliconed over the back cover opening to protect the OLED screen from water.

Add TipAsk QuestionCommentDownload
Step 4: 3D Print Files
These are the STL files for all the related holders and supports. These were all designed to fit the support features. The box for the solenoid has to be modified post printing for the power/relay control ports and the LED hole on the front.

Attachments
download {{ file.name }}esp32holder.stlDownloadView in 3D
download {{ file.name }}floatswitchholder.stlDownloadView in 3D
download {{ file.name }}ph holder.stlDownloadView in 3D
download {{ file.name }}switchholder.stlDownloadView in 3D
download {{ file.name }}tempholder.stlDownloadView in 3D
download {{ file.name }}valvebox.stlDownloadView in 3D
download {{ file.name }}poleholder.stlDownloadView in 3D
Add TipAsk QuestionCommentDownload
Step 5: Water Control
Water Control
Water Control
The 12 volt solenoid was placed into its own custom 3D printed housing which also included a port for separate power and a control line from the feather relay board in the main housing. It also included a small red led that turned on when the solenoid is activated. Regular garden hose can connect with the 3/4 inch openings--dont get the 1/2 inch variety of this --you'll have a hard time finding connectors....

Add TipAsk QuestionCommentDownload
Step 6: Program It
Program It
The code is fairly straightforward. It wrangles a couple of different subroutines and reports them over the Blynk network. If you have worked with Blynk before you know the drill. You have to include all the Blynk software and the connect key for your particular microcontroller and report station. You also have to provide credentials to your Wifi connection. It all works rather beautifully and provides a really easy way to report complicated data without doing much work. You have to set up a series of Blynk mediated timers for each measured sensor. These have to be started and run in a separate subroutine. I have separate ones for pH, temp, water height and time that the solenoid valve remains open--this is to check if the water is on too long without filling the tank--not good. The water height subroutine just takes an average of multiple reading from the voltage divider on the eTape (see previous note--this instrument was wired wrong from factory ....) and then correct the reading with map and constrain functions done with measurements in a water tank at the high and low limits of the tape. The pH subroutine was more complicated. DH Robot included some software for doing the initialization but I could not get it to work at all. You will have to take raw readings from the A2 port with buffers at 4.0 and 7.0 (included in kit) and set these into the "acid value" and "neutral value" in the upper section of the program. It will then identify the slope and the y intercept to calculate all subsequent pH values for you. The pH will have to be recalibrated in the same way about every 2 months to check on it. The temp subroutine is your standard one-wire program. The only activity in the void loop section is to check on the status of the two float switches to determine when to turn on the water and start a timer.

Attachments
download {{ file.name }}finalwaterphtemp.inoDownload
Add TipAsk QuestionCommentDownload
Step 7: Use It
Use It
Use It
Use It3 More Images
On initial trials the machine worked well--having easily adjustable range for the instruments and a water-resistant enclosure made for easier setup in a fast changing environment. It will have to be seen if the distance between the two water level switches proves adequate. The Blynk environment made reporting and control with the cell phone easily done. Direct control over the output relay by phone makes overrides of the system possible when scary water level situations arise. The ease with which you can immediately provide channeled output to as many devices as possible makes the sharing of data with multiple people seamless. Future interests will be with automating the nutrient supply, conductivity testing (known issues with pH metering) and mesh networking with other nodes to measure remote locations in the grow complex.
