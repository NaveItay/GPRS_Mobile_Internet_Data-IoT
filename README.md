# GPRS Mobile Internet Data (IoT)
Live data streams to thingspeak cloud (Arduino & SIM900)

![title](/images/Circuit_Diagram.PNG)

## Project management stages:

- Circuit diagram
- Code
- Receive live data to the cloud

# 
###### Circuit diagram
> Connect DHT sensor and Sim900 to the Arduino controller.
> 
>  
> ![title](/images/arduinoSim900.PNG)

#
###### Code
> * ###### Explanation of AT commands from [m2msupport.net](https://m2msupport.net/m2msupport/atcipsprt-set-prompt-of-when-module-sends-data/).   
>    
>       
>       
>      float h = dht.readHumidity();			// Achieving humidity value
>      float t = dht.readTemperature(); 		// Achieving temperature value
>      delay(100);   
>         
>      Serial.print("Temperature = ");
>      Serial.print(t);
>      Serial.println(" Â°C");
>      Serial.print("Humidity = ");
>      Serial.print(h);
>      Serial.println(" %");    
>      
>   
>  
>      if (gprsSerial.available())
>        Serial.write(gprsSerial.read());
>     
>      gprsSerial.println("AT");				// Handshake test
>      delay(1000);
>     
>      gprsSerial.println("AT+CPIN?");		// sets the password of the mobile device
>      delay(1000);
>     
>      gprsSerial.println("AT+CREG?");		// gives information about the registration status and access technology of the serving cell.
>      delay(1000);
>     
>      gprsSerial.println("AT+CGATT?");		// used to attach or detach the device to packet domain service.
>      delay(1000);
>     
>      gprsSerial.println("AT+CIPSHUT");		// will close the GPRS PDP context.
>      delay(1000);
>     
>      gprsSerial.println("AT+CIPSTATUS");	// returns the current connection status. This command returns the applicable server status, client status, conenction number (for multi-ip) and GPRS bearer info.
>      delay(2000);
>     
>      gprsSerial.println("AT+CIPMUX=0");	// used to start a multi-IP connection.
>      delay(2000);
>   
>      ShowSerialData();
>   
>     prsSerial.println("AT+CSTT=\"uinternet\"");	// Setting the APN,
>      delay(1000);
>     
>      ShowSerialData();
>     
>      gprsSerial.println("AT+CIICR");	// Wireless connection
>      delay(3000);
>     
>      ShowSerialData();
>     
>      gprsSerial.println("AT+CIFSR");	//Local IP adress
>      delay(2000);
>     
>      ShowSerialData();
>     
>      gprsSerial.println("AT+CIPSPRT=0");	// Set whether echo prompt ">" after issuing "AT+CIPSEND" command.
>      delay(3000);
>     
>      ShowSerialData();
>   
>      gprsSerial.println("AT+CIPSTART=\"TCP\",\"api.thingspeak.com\",\"80\"");	// begin the connection
>      delay(6000);
>     
>      ShowSerialData();
>     
>      gprsSerial.println("AT+CIPSEND");	// Live data stream to remote server
>      delay(4000);
>      ShowSerialData();
>      
>      String str="GET https://api.thingspeak.com/update?api_key=Your-Key" + String(t) +"&field2="+String(h);
>      Serial.println(str);
>      gprsSerial.println(str);	// Send data to sim900
>      
>      delay(4000);
>      ShowSerialData();
>     
>      gprsSerial.println((char)26);	
>      delay(5000);	// The time is base on the condition of internet, important! (thingspeak limits)
>      gprsSerial.println();
>     
>      ShowSerialData();
>     
>      gprsSerial.println("AT+CIPSHUT");	// Close the connection
>      delay(100);
>      ShowSerialData(); 
>      
>

# 
###### Receive live data to the cloud
> Receive your data to ThingSpeak from your device.
> 
>  
> ![title](/images/ThingSpeak_Cloud.PNG)
