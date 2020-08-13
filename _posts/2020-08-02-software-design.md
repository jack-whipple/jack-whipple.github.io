---
title: "Software Design/Engineering Artifact"
date: 2020-08-02
tags: [software design, engineering, computer science, raspberrypi, java]
header:
  image: "/images/weatherStation.jpg"
excerpt: "Software Design, Engineering, Computer Science, RaspberryPi, Java"
---

### ->Software Design/Engineering Narrative<-

The purpose of this narrative is to reflect on the artifact I selected for this category and the process I experienced in creating and it. Throughout this narrative I will describe the artifact I selected, explain why its inclusion in my ePortfolio is justified, how I am meeting course outcomes with this artifact, and finally reflect on the process of creating this artifact and share the lessons and challenges I had while working on this project.

##### Describe the Artifact
The artifact I chose for this category is called the WetSpec Temperature embedded system program. This artifact comes from the SNHU class CS 350: Emerging Systems Architectures and Technologies and was originally programmed in the Python scripting language using a Raspberry Pi, and the GrovePi+ sensor kit. I originally created this artifact in 2020. The program uses an analog sensor to get a light value reading and as long as that value is less than a light threshold level, the program will continue to iterate through a while loop. During the loop the program will use a temperature sensor to get temperature and humidity readings and light up LED colored lights based on the current temperature/humidity conditions. The program also records the data in ArrayLists and then writes the data to JSON files so that they can be displayed on charts in a web browser using CanvasJS.

##### Justify Artifact Inclusion
I chose this artifact for my ePortfolio because it aligns with software design/engineering principles. I had to write and develop a program that allowed an embedded system to be functional, serve its purpose, and execute without error. This artifact specifically shows my ability to write code with developed variables that are used in functions and store values, and it shows my ability to write nested functions which can be seen by the IF…ELSE statements contained within a while loop. This artifact also shows my ability to store data in lists and then write those lists to JSON files that are saved as text files in a local folder/directory. The enhancement I made to this artifact is that I rewrote the program in Java. In order to showcase more skills in software development, I decided to transfer this artifact from Python to Java. This enhancement meant having to change data structures and work with Java classes and methods as well as work with a different set of syntaxes.

##### Course Objectives
The enhancement I made to this artifact has met course objectives that I had planned on in the ePortfolio selection journal. The enhancement I implemented demonstrates my ability to program solutions to solve logic problems in software. This was specifically addressed by the code in this program that uses IF…ELSE statements to turn led lights on/off based on the most recent temperature reading. The enhanced artifact also demonstrates my ability to use innovative skills and techniques for implementing design solutions. Using different methods, I was able to write blocks of code that could be used/executed later when called so that I could control when the different methods executed. This allowed me to keep my code clean and efficient. I have included more coverage in potential design flaws related to security by including more exceptions, but there could be more addressed in this area.

##### Artifact Reflection
I have learned a lot while making the enhancement to this artifact. While going through the enhancement process I have continued to learn more about Java than I previously knew. My knowledge on Java classes, use of methods and how to call them, and the use of Java ArrayLists has expanded even more. I have a much better understanding on how to create a method that does not run until called upon in another method. I also understand that Java Arrays are finite and only hold a specified number of elements; whereas Java ArrayLists are infinite, and elements can always be added onto the end of the list. I encountered a few challenges, especially while trying to merge ArrayLists into one list that kept elements as key value pairs. Another challenge was trying to store temperature and humidity readings from the sensor into their respective variables. Even writing Java ArrayLists to JSON files was a bit challenging. Overall, I learned that the GrovePi is very developer friendly when working with Python. The Python version of this program was much more straightforward to write and easier to develop. The Java version of this project requires much more complicated syntax and data structure but can be more efficient in its execution of the program. Personally, I learned that I am able to develop in Java better that I though I could, but I still need more practice in order to become a better developer in Java.


### Software Design and Engineering Artifact:
```js
/*
 * **********************************************************************
 * PROJECT       :  GrovePi WetSpec Weather Station
 *
 * This file is part of the GrovePi Java Library project. More information about
 * this project can be found here:  https://github.com/DexterInd/GrovePi
 * **********************************************************************
 * 
 * ## License
 * 
 * The MIT License (MIT)
 * GrovePi for the Raspberry Pi: an open source platform for connecting Grove 
 * Sensors to the Raspberry Pi.
 * 
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 * THE SOFTWARE.
 */

package org.iot.raspberry.grovepi;

import java.util.*;
import java.lang.Math;
import java.time.LocalDateTime;
import java.io.*;
import javafx.util.Pair;
import org.json.simple.JSONArray; // Stores an array of JSON objects

import org.iot.raspberry.grovepi.GroveDigitalOut;
import org.iot.raspberry.grovepi.GrovePi;
import org.iot.raspberry.grovepi.devices.GroveLightSensor;
import org.iot.raspberry.grovepi.devices.GroveTemperatureAndHumiditySensor;

/*
 * WetSpec Weather Station Code
 * The WetSpec weather station is designed to run as an embedded system from the
 * Raspberry Pi 3. This application is designed to function using multiple
 * sensors and I/O devices from the GrovePi kit. From GrovePi the sensors and 
 * I/O devices are, the Grove Temperature & Humidity Sensor, Grove Light Sensor
 * Pro, and Grove LED Socket Kits (Red, Green, & Blue).
 *
 * Created 6/20/2020
 * @author Jack Whipple
 */

public class wetSpec_weatherStation {
    
    // Declare Variables
    protected int resistance;
    protected int temp;
    protected int humid;
    protected long time;
    protected int reading;
 
    /*
     * WetSpec will record data as long as sensor does not exceed threhold
     * resistance. Set threshold value to 10
     */ 
    protected int lightThreshold = 10;
    
    /*
	   * Use WetSpec Digital Sensor to get temperature and humidity vlaues
	   */
	 
	  public void getReading(GrovePi grovePi, int temp, int humid) throws IOException{
        /*
         * Connect Temperature Sensor to port D4 and tell system what type of GrovePi
         * temperature sensor is being used
         */
        GroveTemperatureAndHumiditySensor dht = new GroveTemperatureAndHumiditySensor(grovePi, 4, GroveTemperatureAndHumiditySensor.Type.DHT11);
        
        tempReading = dht.get();   
    } 
		 
	
	  /*
     * The following method will be called later in the "run" method
     * Write data into ArrayLists and Pair list in order to combine data so it can
     * be written into a JSON file
     */ 
    public void dataLists() throws IOException{
        ArrayList<Integer> tempList = new ArrayList<Integer>();
        tempList.add(temp);
        
        ArrayList<Integer> humidList = new ArrayList<Integer>();
        humidList.add(humid);
        
        ArrayList<Long> timeList = new ArrayList<Long>();
        timeList.add(time);
        
        ArrayList <Pair <Long,Integer> > timeTempList = new ArrayList <Pair <Long,Integer> >();
        timeTempList.add(new Pair <Long,Integer> (timeList, tempList));
        
        ArrayList <Pair <Long,Integer> > timeHumidList = new ArrayList <Pair <Long,Integer> >();
        timeHumidList.add(new Pair <Long,Integer> (timeList, humidList));
        
        JSONArray jsTemp = new JSONArray(timeTempList);
        JSONArray jsHumid = new JSONArray(timeHumidList);
        
        /*
         * Write JSONArrays to JSON file
         */
        FileWriter jsWriteTemp = new FileWriter("/srv/sftp/shared/java/timeTempList.txt");
        jsTemp.writeJSONString(jsWriteTemp);
        jsWriteTemp.close();
        
        FileWriter jsWriteHumid = new FileWriter("/srv/sftp/shared/java/timeHumidList.txt");
        jsHumid.writeJSONString(jsWriteHumid);
        jsWriteHumid.close();
    }
        
    /*
     * Add inital start up message for user when the program executes
     */
    public void statement(){
          System.out.println("Welcome to the WetSpec embedded system on the Raspberry Pi 3 device utilizing GrovePi sensors.");
          System.out.println("This device will record temperature and humidity readings once every half hour.");
          System.out.println("The readings gather from this program will be stored in JSON files where they can be displayed on other systems through the canvasJS dashboard.");
          System.out.println(" ");
    }
    
    /*
     * Display in the terminal when the program starts using local date and time
     */
    public void programStart() {
            LocalDateTime programStart = LocalDateTime.now();
            System.out.println(programStart);
    }
    
    public void run(String[] args, GrovePi grovePi) throws Exception {
        statement();
        programStart();
        
        /*
         * Connect Light Sensor to port A0
         * Connect Led lights to D3, D7, & D8 (Red, Green, Blue)
         */
        GroveLightSensor lightSensor = new GroveLightSensor(grovePi, 0);
        GroveDigitalOut redLed = new GroveDigitalOut(grovePi, 3);
        GroveDigitalOut greenLed = new GroveDigitalOut(grovePi, 7);
        GroveDigitalOut blueLed = new GroveDigitalOut(grovePi, 8);
        
        /*
         * Have light sensor get value and calcualate resistance. Run through loop
         * as long as resistance value is less than threshold.
         */
        int lightValue = lightSensor.get();
        resistance = (1023 - lightValue) * 10 / lightValue;
        while (true) {
            if (resistance < lightThreshold) {
                try {
                        // Return values for temp and humid and convert temp from C to F
                        getReading();
                        temp = (int) (Math.round((temp/5.0)*9) + 32); // Convert temperature from celsius to fahrenheit
                        String tempFormat = String.format("%.02f", temp); // Format temp reading for terminal window
                        String humidFormat = String.format("%.02f", humid); // Format humid reading for terminal window
                        System.out.println("temp = " + tempFormat + "F " + "humidity = " + humidFormat + "%");
                    
                        Date date = new Date(); // Get current date to use for time variable
                        time = date.getTime(); // returns time in milliseconds
                    
                        /*
                         * If...Else statement to turn LED lights on/off based on 
                         * temperature conditions
                         */
                        boolean on = true; // if light is on, then value is true
                        if (60 < temp && temp < 85 && humid < 80){           // Turn on green led
                            redLed = !on;
                            greenLed = on;
                            blueLed = !on;
                        }else if (85 < temp && temp < 95 && humid < 80) {    // Turn on blue led
                            redLed = !on;
                            greenLed = !on;
                            blueLed = on;
                        }else if (temp > 95) {                               // Turn on red led
                            redLed = on;
                            greenLed = !on;
                            blueLed = !on;
                        }else if (humid > 80) {                              // Turn on green and blue leds
                            redLed = !on;
                            greenLed = on;
                            blueLed = on;
                        }else {                                              // Turn off all leds     
                            redLed = !on;
                            greenLed = !on;
                            blueLed = !on;
                            System.out.println("Temperature readings outside of expected range.");
                        }
                }catch (Exception e) {
                    System.out.println("Program error. Check Sensors and LEDs.");
                }
            }else {
                boolean off = true; // if light is off, value is true
                redLed = off;
                greenLed = off;
                blueLed = off;
                System.out.println("Program terminated. WetSpec doesn't record at night.");
                break;
            }
         
            /*
             * This section will write time, temperature, and humidity data into the ArrayLists.
             * The data in these lists will be written to JSON files in order to be used with CanavasJS.
             */
            dataLists();
            
            Thread.sleep(1800000); // Run while loop once every 30 mintues (1,800,000 milliseconds) to get a reading twice per hour    
        }   
  }
}
```
