# Heewing T01 Ranger VTOL (Kakute H7 mini) Build Log
The Heewing T01 Ranger is a twin-motor RC plane that comes as a plug-and-play kit. The following document 
explains the additional steps required to convert the standard plane into a fully functioning VTOL.

## Bill of materials
### Kit Hardware
* [Heewing T01 Ranger](https://www.heewing.com/products/heewing-ranger-t-1-fpv-airplane-730mm-wingspan-epp-with-flight-controller-pnp-pro)
* [Heewing T01 Ranger VTOL Conversion Kit](https://www.heewing.com/products/hee-wing-t1-ranger-vtol-conversion-kit)
  * Note, this build was modified to feature motors and ESCs capable of handling a 6s battery. 
### Electronics
* [3 x 2203 1500 KV Motor](https://stanfpv.com/products/stan-fpv-2203-1500kv-pro-motor)
* [Lumenier Razor Pro 45A ESC](https://www.getfpv.com/lumenier-razor-pro-f3-blheli-32-45a-2-6s-esc.html?utm_source=google&utm_medium=cpc&utm_campaign=DM+-+NB+-+PMax+-+Shop+-+SM+-+ALL&utm_content=pmax_x&utm_keyword=&utm_matchtype=&campaign_id=19697845436&network=x&device=c&gclid=EAIaIQobChMIj73bk4Sg_QIVeQytBh3PZQetEAQYASABEgL_YvD_BwE)
* [Kakute H7 mini Flight Controller](https://shop.holybro.com/kakute-h7-mini_p1308.html)
* Walksnail Avatar mini VTX
* [Mateksys Power Distribution Board](https://www.getfpv.com/mateksys-servo-pdb-w-bec-5-5-36v-to-5-8-2v-svpdb-8s.html)
* [FRSky Archer RS receiver](https://www.frsky-rc.com/product/archer-rs/)
* MRO Telemetry
* MRO GPS
### Tools needed
The following tools were used in this assembly
* Soldering iron
* Screwdriver Set

## Assembly
1. Start by assembling the Heewing T01 Ranger
   * Out of the box the plane does have some assembly required including installing the tail, wings, and screwing in adapter plates.
The [assembly guide](https://cdn.shopifycdn.net/s/files/1/0553/6573/0348/files/T1_PNP_Assembly_Guide.pdf?v=1640164559) released by Heewing
should be used to get through the initial setup of the plane. 
2. Continue by applying the VTOL modifications. The VTOL conversion would require installing new higher-powered motors, a rear motor, and tilt servos. Heewing has released [instructional videos](https://www.heewing.com/products/hee-wing-t1-ranger-vtol-conversion-kit) on how to accomplish this.
   
### Wiring
* The following chart shows the pinout assignments of the flight controller. Refer to the [pinout for the Kakute H7 mini](https://docs.holybro.com/fpv-flight-controller/kakute-h7-mini/pinout) for more details


| Port | Connection       |
|-----:|------------------|
| Main1|Front Left Motor  |
| Main2|Front Right Motor |
| Main3|Rear Motor        |
| Main5|Attatched Ailerons|
| Main6|Right Tilt Servo  |
| Main7|Left Tilt Servo   |
| Main8|Elevator          |
| UARTX|Telemetry         |
| UARTX|Reciever          |
|   I2C|GPS               |

* The image below shows a suitable configuration for wiring. From the flight controller, wires are going from the I2C into the GPS, from UART 1 to the 
telemetry, from the RC to the receiver, and from M5 - M8 to a PDB.
* The PDB's outputs are connected to the signal, power, and ground wire of each motor.  

![WiringFC](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/0d71d840-cfc5-445a-b788-38d28eca3169)


* The Heewing kit comes with a BEC attached to two quick-release buses that power the Motors. This BEC has also been soldered onto to power the
flight controller, PDB, rear ESC/motor, and VTX.  


![PDBWiringDiagram](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/87c2025d-c730-4a0c-a8af-0ef349c18094)



## PX4 Configuration
* This section will go over what parameters have been modified on this build and why. More information is available [here](https://docs.px4.io/main/en/config/actuators.html)
### Actuator Outputs
1. Within the "Actuator Outputs" plane assign which actuator corresponds to which output on the flight controller. Referring back to the wiring assignment shows that M1 is paired with the front left motor or Motor 1, M2 to the front right motor or Motor 2, M3 to the rear motor or Motor 3, and so on. 
#### MC Motors
1. Starting from the geometry section of the actuators menu, set the value to 3.
2. The X position indicates how far forward or backward the motor is from the center of gravity, forward being positive and backward being negative. Conversely, the Y position indicates how far left or right the motor is in regards to the center of gravity, with left being negative and right being positive.
3. Declare which motor is being tilted by which servo. This can be modified by the drop-down menu following the X and Y position.
4. Changing the direction of CCW will indicate in what direction the motors are spinning. By default, it will indicate counterclockwise with respect to the FRD coordinate system around PX4FMU's Z axis or Yaw axis. For more information on the FRD coordinate system see the [PX4 Terminology page](https://docs.px4.io/main/en/contribute/notation.html)  
![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/b9510f60-84d5-42b6-80e2-de7e818d7d62)  

#### Control Surfaces
   * The number of control surfaces can be modified by using the drop-down menu labeled "Control Surfaces".
1. The VTOL uses two control surfaces which include a single-channel aileron and an elevator.
![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/f7888395-c1ad-4173-8ca6-924d6846bb72)  

#### PWM
1. Set the PWM minimum value to correspond to the angle at which the blade barely misses the foam wing.
2. Set the maximum value to correspond to the angle directly forward of the plane.
3. Set the disarm value so that when disarmed the motor is facing straight up.  
![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/993e17e6-e1ed-4c26-8d37-da0aa8139ffe)  

#### Tilt Servos
1. Set the number of tilt servos to two.
2. Using a protractor, measure the angle of the tilt servo when PWM is at its minimum value. Enter the obtained angle into the "Angle at minimum tilt" parameter taking into account that 0° in the PX4 coordinate system is straight up.
3. Do the same for the PWM's maximum value and enter the obtained angle into the "Angle at maximum tilt" parameter.
   * Setting the angle at min and max tilt values defines the usable PWM range of the tilt motor in degrees, 90° being forward, 0° being straight up, and -90° being backward.  
   * The example below shows what the tilt parameter's full range of motion would look like against a Heewing tilt servo. In the example, the full range of motion has a total of 107° of freedom.

![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/29d4ad1a-d3ca-4543-b36b-55729ed5380f)
![rotationdiagram](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/704d713d-8f5d-4c8d-98a9-6ec37b909c78)



#### Transition and MC Mode Angles
* Before flying, the tilt servo positions during MC Mode and Transition Mode must be declared.

![VTOLTILTANGLE](https://user-images.githubusercontent.com/117425577/220211260-bbadd5ad-7194-4f5b-94d3-57c7c9989fd9.png)
1. To declare the motor's position during MC Mode, assign the VT_TILT_MC parameter. The input value should be a percentage of the tilt servo's full range starting from the angle at minimum tilt.
* The image below shows an example in which the angle of transition is at 13.5% of the tilt servo's full range of motion. Note that the angle corresponding to 13.5% is slightly less than 90°. This is to compensate for a center of gravity that is too far forward.
![MCMode](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/0419e594-8cf5-4a09-a096-555fb7534519)



2. To declare the motor's position during the transition, assign the VT_TILT_TRANS parameter. Similar to the VT_TILT_MC parameter, the input is a percentage of the tilt servo's full range starting from the angle at minimum tilt. 
* The image below shows an example in which the angle of transition is at 80% of the tilt servo's full range of motion.

![Transition](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/055569fc-579f-4236-af71-844f979c4e0c)

### Airspeed Sensor
* The airspeed sensor has been disabled due to bugs in the latest version of PX4.
