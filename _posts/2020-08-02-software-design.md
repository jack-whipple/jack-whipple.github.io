---
title: "Software Design/Engineering Artifact"
date: 2020-08-02
tags: [software design, engineering, computer science, raspberrypi, java]
header:
  image: "/images/weatherStation.jpg"
excerpt: "Software Design, Engineering, Computer Science, RaspberryPi, Java"
---

Greetings!

This project is my Software Design and Engineering Artifact for CS 499. This is a weather station project that utilizies the GrovePi+ starter kit for the Raspberry Pi 3. This project was originally created by me in the Python scripting language and serves the purpose of reading temperature and humidity level around the weather station. During program execution, the light sensor will first detect light levels to determine if daylight conditions are true. If conditions are true, the program will read the input from the GrovePi digtial temperature sensor and print the readings to the terminal window and save the data to JSON files. Additionally, GrovePi LED lights are used as visual cues to the current weather conditions. The light sensor is designed to check daylight conditions once every 30 minutes. Once daylight conditions are found to be false, the program will terminate.

For the CS 499 Software Design and Engineering artifact enhancement, I chose to rewrite this program in a different language. As I mentioned, this program was orginally written in Python, but Below you will find the Java code used to write the program.

### Software Design and Engineering Artifact:
```java
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
