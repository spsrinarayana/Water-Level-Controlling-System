# Water Level Controlling System.
    The submission for the Lab 4 Report of Group 7 for MicroComputers, Y2 S1, Faculty of Engineering, SLIIT 
                   EN21475368 - Sri Narayana S. N. B. M. S. P.
                   EN21488306 - Nilaweera W. N. R. T. R.
                   EN21474132 - Rathnayaka M. H. M.

## Introduction
<p align="justify">
   In this lab, the main objective is to create an Automatic water level controlling system of a water tank using Microcontroller. So, in our real implementation we have used  water detecting sensors, PCB, 2 x Motors, PIC16F877A Microcontroller, 20 MHz Crystal Oscillator, 2 x 2 pF Capacitors. for this Implementation we Programmed Using MicroC language. 
</font>	
</p> 
<p align="justify">
  This System works according to the Combinations of the 3 x Sensor states (Logic High or Logic Low). Water Level sensors we used are Analog Sensors. But, we are using three sensors at three different levels of the Tank. Then, that sensors act as digital sensors in our Implementation. When using those Sensors the Logic low (0) is considered when water level not reaches to the water sensor and the Logic High (1) is considered when water level exceeds the limit of the maximum point of a each individual sensor. Then we can neglect the analog behavour of the Sensor.
</p> 
<p align="justify">
   When designign the PCB we used The Proteus Simulation Software. After, designing the Circuit diagram on Schematic capture in Proteus there is an Option  (PCB layout) to Design the PCB. Next, the Designed PCB layout can be Printed in to a .pdf (Portable Document Format) file. To Design the PCB physically, some steps shoud be followed.
</p>           
                
           
		   		
      

## PCB Design
This is the Implementation of the PCB in PCB layout mode in Proteus Simulation Software when designing the PCB.
<p align="center">
<img src= "https://user-images.githubusercontent.com/47419680/179459428-0e6e42df-8826-4595-a07d-1b5b051771f2.png" width="500" )
</p>
<br>	
<p align="left">
This is the Final Print pdf of the PCB design which used to make the PCB.
</p>
<p align="center">	
<img src= "https://user-images.githubusercontent.com/47419680/179490918-b68f161f-03db-4c31-9df2-629813773f2e.png" width="500" )

<br>
</p>
	
## Image of the Real Implementation 
<p align="justify">
This is the circuit which made on a bread board to design the Final PCB. Before we make the PCB, we have to decide exact components which we are going to used to implement the System. Then, we have to set up all the components on the Bread board  and finalize the Components.  
</p> 

<p align="center">	
<img src= "https://user-images.githubusercontent.com/47419680/179498142-23918b85-86b2-41f8-9e32-ba2ecacffc42.png" width="500" )
</p>
<br>
<p align="justify">
This is How the PCB looks like after it Printed on the Copper board before solder the Components to it.
</p> 
<p align="center">	
<img src= "https://user-images.githubusercontent.com/47419680/180139177-6516c988-3be2-45d2-9f03-a492b72231c6.jpg" width="500" )
</p>
<br> 
<p align="center">	
<img src= "https://user-images.githubusercontent.com/47419680/180152690-d22f8e52-3c39-4bf7-b26b-629bc916e9a3.png" width="500" )
</p>
<br>	
<p align="justify">
The Final System looks like below Video. Here, 2LED's indicates the signal which gives to the Moters. From that Signal we can Operate the Moter according to the Combinations of the Three sensors. 	
</p> 	
<p align="center">	
<video src="https://user-images.githubusercontent.com/47419680/180144178-49a30b7e-b139-45da-a8ff-3b2eb3f40a90.mp4" width="500")
</p>
	
	

## Results
<p align="justify">	
In our design there are three sensors that can give 8 (2<sup>3</sup> combinations) possible combinations. Here, we are given to consider only 3 combinations of the Sensors. Then for other combinations 2 motors should turned off. 
</p> 

###### Combination 1
	Sensor 1 (sw1) is ON
	Sensor 2 (sw2) is OFF
	Sensor 3 (sw3) is OFF
	Then,
		Motor 1 - ON
		Moter 2 - OFF
###### Combination 2
	Sensor 1 (sw1) is ON
	Sensor 2 (sw2) is ON
	Sensor 3 (sw3) is OFF
	Then,
		Motor 1 - ON
		Moter 2 - OFF
###### Combination 3
	Sensor 1 (sw1) is ON
	Sensor 2 (sw2) is ON
	Sensor 3 (sw3) is ON
	Then,
		Motor 1 - OFF
		Moter 2 - ON (for 500 ms)
		Moter 2 - OFF
###### Other combinations
<p align="justify">	
When water is filling into the tank, other combinations (other than above 3 Combinations) are not possible because water is filled from the tank bottom to top. At an occasion which water stucked in a sensor, the two motors should not work. Therefore, In other combinations of the Sensors,
</p> 	
<br> 	
	<p align="center">
	Motor 1 - OFF <br>
		Moter 2 - OFF
    	</p>

## Code

    // PIC16F877A Configuration Bit Settings

    // 'C' source line config statements

    // CONFIG
    #pragma config FOSC = HS        
    #pragma config WDTE = OFF       
    #pragma config PWRTE = OFF      
    #pragma config BOREN = OFF      
    #pragma config LVP = OFF        
    #pragma config CPD = OFF        
    #pragma config WRT = OFF         
    #pragma config CP = OFF         



    // The PINS We Used In PIC16F877A
    //      Switch 1 is connected to RB2
    //      Switch 2 is connected to RB1
    //      Switch 3 is connected to RB0
    //      Motor 1 is connected to RC0
    //      Motor 2 is connected to RC1



    #include <xc.h>
    #include <htc.h> 

    #define _XTAL_FREQ 20000000

    void __interrupt() isr(void) {      // When RB0=1 interrupt function executed
       if(RB2 == 1 && RB1 == 1) {      // also with RB0=1 other 2 switches also to be Logic high to proceed
           RC0 = 0;                    //Motor1 is turned off
            RC1 = 1;                    //Motor2 is turn on and again off after 1/2 second
            __delay_ms(500);
            RC1 = 0;        
        }
        INTF = 0;
    
    }
    void main(void){
        GIE = 1;            //Global Interrupt Enable
        INTE = 1;           //RB0/INT pin interrupt Enable
        INTF = 0;           //Interrupt Flag to be logic low. if interrupt pin at logic HIGH the flag become High to call interrupt
        PEIE = 1 ;          //Peripheral Interrupt Enable
        INTEDG = 1;         
    
        TRISB0 = 1;     // SWITCH 3 , set as an Input
        TRISB1 = 1;     // SWITCH 2 , set as an Input
        TRISB2 = 1;     // SWITCH 1 , set as an Input
        TRISC0 = 0;     // MOTOR 1 , set as an Output
        TRISC1 = 0;     // MOTOR 2 , set as an Output
        PORTB = 0X00;   // all portB set to Logic Low
        PORTC = 0X00;   // all portC set to Logic Low
      
        while(1){                       //Infinity While loop running inside the main Program Until Interrupt Occurs
            if(RB2 == 1 && RB0==0){     // If condition Run when Switch1 at logic High, Switch 3
                RC0 = 1;                // if RB2=1, Motor 1 is ON and Motor 2 is off
                RC1 = 0;                // the same combination for the both ON/OFF fro the Switch 2 (RB1),
            }else{                      // If the above combinations not true 
                RC0 = 0;                // Both Motors are Turned off
                RC1 = 0;
           }
                                
        }
        return;
    }
