# Heewing T01 Ranger VTOL (Kakute H7 mini) Build Log
The Heewing T01 Ranger is a twin-motor RC plane that comes as a plug-and-play kit. The following document 
explains the additional steps required to convert the standard plane into a fully functioning VTOL.

## Bill of materials

### Kit Hardware
* [Heewing T01 Ranger](https://www.heewing.com/products/heewing-ranger-t-1-fpv-airplane-730mm-wingspan-epp-with-flight-controller-pnp-pro)
* [Heewing T01 Ranger VTOL Conversion Kit](https://www.heewing.com/products/hee-wing-t1-ranger-vtol-conversion-kit)
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
* X-ACTO blade
* Screwdriver Set

## Assembly
1. Start by assembling the Heewing T01 Ranger
   * Out of the box the plane does have some assembly required including installing the tail, wings, and screwing in adapter plates.
The [assembly guide](https://cdn.shopifycdn.net/s/files/1/0553/6573/0348/files/T1_PNP_Assembly_Guide.pdf?v=1640164559) released by Heewing
should be used to get through the initial setup of the plane. 
2. Continue by applying the VTOL modifications. The VTOL conversion would require installing new higher-powered motors, a rear motor, and tilt servos. Heewing has released (instructional videos)[https://www.heewing.com/products/hee-wing-t1-ranger-vtol-conversion-kit] on how to accomplish this.
   
### Wiring
* The following chart shows the pinout assignments of the flight controller. Refer to the [pinout for the Kakute H7 mini](https://docs.holybro.com/fpv-flight-controller/kakute-h7-mini/pinout) for more details


| Port | Connection       |
|-----:|------------------|
| Main1|Front Left Motor  |
| Main2|Front Right Motor |
| Main3|Rear Motor        |
| Main5|Elevator          |
| Main6|Attatched Ailerons|
| Main7|Left Tilt Servo   |
| Main8|Right Tilt Servo  |
| UARTX|Telemetry         |
| UARTX|Reciever          |
|   I2C|GPS               |

* The image below shows a suitable configuration for wiring. From the flight controller, there are wires going from the I2C into the GPS, from UART 1 to the 
telemetry, from the RC to the receiver, and connecting M5 - M8 to a PDB.
![IMG_5527](https://user-images.githubusercontent.com/117425577/219988439-aa2120e9-12dd-4a75-89a7-9e9a51257035.jpg)





* The Heewing kit comes with a BEC attached to two quick-release buses that power the Motors. This BEC has also been soldered onto to power the
flight controller, pdb, rear ESC/motor, and VTX.

![Wiring2](https://user-images.githubusercontent.com/117425577/220202423-3d94a367-2aad-4e95-af08-018184116720.jpg)



## PX4 Configuration
* This section will go over what parameters have been modified on this build and why. More information is available [here](https://docs.px4.io/main/en/config/actuators.html)
## Actuator Outputs
1. Within the "Actuator Outputs" plane assign which actuator corresponds to which output on the flight controller. Refering back to the wiring assignment shows that M1 is paired with the front left motor or Motor 1, M2 to the front right motor or Motor 2, M3 to the rear motor or Motor 3, and so on. 
### MC Motors
1. Starting from the geometry section of the actuators menu, set the value to 3.
2. The X position indicates how far forward or backward the motor is from the center of gravity, forward being positive and backward being negative. Conversely, the Y position indicates how far left or right the motor is in regards to the center of gravity, with left being negative and right being positive.
3. Declare which motor is being tilted by which servo. This can be modified by the drop-down menu following the X and Y position.
4. Changing the direction CCW will indicate in what direction the motors are spinning. By default, it will indicate counterclockwise with respect to the FRD coordinate system around PX4FMU's Z axis or Yaw axis. For more information on the FRD coordinate system see the [PX4 Terminology page](https://docs.px4.io/main/en/contribute/notation.html)  
![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/b9510f60-84d5-42b6-80e2-de7e818d7d62)  

### Control Surfaces
   * The VTOL uses two control surfaces which include a single-channel aileron and an elevator. We are using a single channel aileron due to the limited number of output channels on the flight controller.
1. The number of control surfaces can be modified by using the drop-down menu labeled "Control Surfaces".
2. If you look at the "Actuator Outputs" section you can see how the servos correspond to the flight controller's outputs.  

![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/f7888395-c1ad-4173-8ca6-924d6846bb72)  

### PWM
1. Set the PWM maximum value to correspond to the angle at which the blade barely misses the foam wing.
2. Set the minimum value to correspond to the angle directly forward of the plane.
3. Set the disarm value so that when disarmed the motor is facing straignt up.  
![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/993e17e6-e1ed-4c26-8d37-da0aa8139ffe)  

### Tilt Servos
1. Set the number of tilt servos to two.
2. Set the angle at minimum tilt to the angle corresponding to the minimum PWM value.
3. Set the angle at maximum tilt to the angle corresponding to the maximum PWM value. 
   * By setting the angle at min and max tilt values you are defining the usable PWM range of the tilt motor in degrees, 90 being forward, 0 being straight up, and -90 being backward.  
   * The example below's full range of motion is 107°.

![image](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/29d4ad1a-d3ca-4543-b36b-55729ed5380f)
![TiltServ](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/a685d131-4c25-42d9-a990-29ffcfba1133)


### Transition and MC Mode Angles
Before flying, the tilt servo's position during MC Mode and Transition Mode must be declared.

![VTOLTILTANGLE](https://user-images.githubusercontent.com/117425577/220211260-bbadd5ad-7194-4f5b-94d3-57c7c9989fd9.png)
1. To declare the motor's position during MC Mode, assign the VT_TILT_MC parameter. The input value should be a percentage of the tilt servo's full range starting from the angle at minimum tilt.
* The image below shows an example in which the angle of transition is at 13.5% of the tilt servo's full range of motion. Note that the angle corresponding to 13.5% is slightly less than 90°. This is to compensate for a center of gravity that is too far forward.
![Screenshot 2023-05-16 141806](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/eb4610c9-bae7-446c-b3a3-8cb394fcd563)


2. To declare the motor's position during transition, assign the VT_TILT_TRANS parameter. Similar to the VT_TILT_MC paramter, the input is a percentage of the tilt servo's full range starting from the angle at minimum tilt. 
* The image below shows an example in which the angle of transition is at 80% of the tilt servo's full range of motion.

![MCMode](https://github.com/arguelle/VTOL-at-UNLV/assets/117425577/97c083da-edd1-458e-b04a-a181d58ba79f)


°








