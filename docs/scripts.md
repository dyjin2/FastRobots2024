PART 1
Hook the Artemis board up to your computer, and follow the instructions from bulletpoint 2 above.

From the setup instructions linked above, follow the instructions in “Example: Blink it Up”. 

In File->Examples->Artemis Examples, run Example4_Serial.

In File->Examples->Artemis Examples, run Example2_analogRead to test your temperature sensor. Try blowing on or touching the chip to change its temperature.

In File->Examples->PDM, run Example1_MicrophoneOutput to test your microphone. E.g. try whistling or speaking to change the highest frequency.

Program the board to turn on the LED when you play a musical “A” note over the speaker, and off otherwise. Use your phone, computer, or similar to generate the sound. If you’re having fun you could even combine the microphone and the Serial output to generate an electronic tuner.

PART 2

Send an ECHO command with a string value from the computer to the Artemis board, and receive an augmented string on the computer. For example, the computer sends the string value “HiHello” to the Artemis board using the ECHO command, and the computer receives the augmented string “Robot says -> HiHello :)” from a read GATT characteristic.

Add a command GET_TIME_MILLIS which makes the robot reply write a string such as “T:123456” to the string characteristic.


Setup a notification handler in Python to receive the string value (the BLEStringCharactersitic in Arduino) from the Artemis board. In the callback function, extract the time from the string. I wrote a notification handler.

Write a loop that gets the current time in milliseconds and sends it to your laptop to be received and processed by the notification handler. Collect these values for a few seconds and use the time stamps to determine how fast messages can be sent. What is the effective data transfer rate of this method?

Now create an array that can store time stamps. This array should be defined globally so that other functions can access it if need be. In the loop, rather than send each time stamp, place each time stamp into the array. (Note: you’ll need some extra logic to determine when your array is full so you don’t “over fill” the array.) Then add a command SEND_TIME_DATA which loops the array and sends each data point as a string to your laptop to be processed. (You can store these values in a list in python to determine if all the data was sent over.)

Add a second array that is the same size as the time stamp array. Use this array to store temperature readings. Each element in both arrays should correspond, e.e., the first time stamp was recorded at the same time as the first temperature reading. Then add a command GET_TEMP_READINGS that loops through both arrays concurrently and sends each temperature reading with a time stamp. The notification handler should parse these strings and add populate the data into two lists.

Discuss the differences between these two methods, the advantages and disadvantages of both and the potential scenarios that you might choose one method over the other. How “quickly” can the second method record data? The Artemis board has 384 kB of RAM. Approximately how much data can you store to send without running out of memory?

Effective Data Rate And Overhead: Send a message from the computer and receive a reply from the Artemis board. Note the respective times for each event, calculate the data rate for 5-byte replies and 120-byte replies. Do many short packets introduce a lot of overhead? Do larger replies help to reduce overhead? You may also test additional reply sizes. Please include at least one plot to support your write-up.

Reliability: What happens when you send data at a higher rate from the robot to the computer? Does the computer read all the data published (without missing anything) from the Artemis board? Include your answer in the write-up.

Discussion