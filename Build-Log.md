# Heewing T01 Ranger VTOL (Kakute H7 mini) Build Log
The Heewing T01 Ranger is a twin-motor RC plane that comes as a plug-and-play kit out of the box. The following document 
goes over the additional steps required to convert the standard plane into a fully functioning VTOL.
## Key information
The Heewing VTOL kit does come with all of the required materials needed to have a sufficient VTOL (minus a flight controller and receiver), however, this
build log goes over all of the materials we are using in our current build which includes alternative motors and ESCs.


## Bill of materials

### Kit Hardware
* [Heewing T01 Ranger](https://www.heewing.com/products/heewing-ranger-t-1-fpv-airplane-730mm-wingspan-epp-with-flight-controller-pnp-pro)
	- EPP Foam body, wings, and tail
	- Carbon Fiber spar
* [Heewing T01 Ranger VTOL Conversion Kit](https://www.heewing.com/products/hee-wing-t1-ranger-vtol-conversion-kit)
  - Tilt Servos and Mounts
  - Tail Boom
  - Rear Motor Mount
  - Propellers
### Electronics
* [3 x 2203 1500 KV Motor](https://stanfpv.com/products/stan-fpv-2203-1500kv-pro-motor)
* [Lumenier Razor Pro 45A ESC](https://www.getfpv.com/lumenier-razor-pro-f3-blheli-32-45a-2-6s-esc.html?utm_source=google&utm_medium=cpc&utm_campaign=DM+-+NB+-+PMax+-+Shop+-+SM+-+ALL&utm_content=pmax_x&utm_keyword=&utm_matchtype=&campaign_id=19697845436&network=x&device=c&gclid=EAIaIQobChMIj73bk4Sg_QIVeQytBh3PZQetEAQYASABEgL_YvD_BwE)
* [Kakute H7 mini Flight Controller](https://shop.holybro.com/kakute-h7-mini_p1308.html)
* Walksnail Avatar mini VTX
* [Mateksys Power Distribution Board](https://www.getfpv.com/mateksys-servo-pdb-w-bec-5-5-36v-to-5-8-2v-svpdb-8s.html)
* [FRSky Archer RS receiver](https://www.frsky-rc.com/product/archer-rs/)
* MRO Telemetry
* MRO GPS
* Pico Blade Connectors
* JSTGH Connectors
### Tools needed
The following tools were used in this assembly
* Soldering iron
* X-ACTO blade
* Size 1 screwdriver

## Assembly
1. Start by assembling the Heewing T01 Ranger
   * Out of the box the plane does have some assembly required including installing the tail, wings, and screwing in adapter plates.
The [assembly guide](https://cdn.shopifycdn.net/s/files/1/0553/6573/0348/files/T1_PNP_Assembly_Guide.pdf?v=1640164559) released by Heewing
should be used to get through the initial setup of the plane. 
2. Continue by applying the VTOL modifications. VTOL conversion kit would require installing new higher-powered motors, a rear motor, and tilt servos. This build log does go over it however Heewing has released (instructional videos)[https://www.heewing.com/products/hee-wing-t1-ranger-vtol-conversion-kit] on how to accomplish this.
   * Remove both motors and motor mounts and desolder the existing ESCs to replace them with your new ESCs.
   * Solder the new motors onto the new ESCs and install the tilt servos.
   * Slide the rear motor mount through the carbon fiber tail boom and push the motor wires through the hole on the lower end of the pipe.
   * Solder bullet connectors onto the rear motor wires poking through the end of the tail boom (optionally you can solder the wires directly to the ESC).
   * Solder bullet connectors onto the ESC for the rear motor.
   * Slide the tail boom onto the fuselage of the plane and secure using the red nut.
3. Mount VTX onto the top front hatch of the fuselage.
   * Carefully cut away foam from the front hatch of the plane using the VTX as a guide. Once enough foam has been removed the VTX should fit snuggly into the hole. Be sure to leave room for the power and ground wires
   * Using a hot iron poke two holes into the side of the fuselage angled downward for the VTX antennas to come out of.
![VTXmount](https://user-images.githubusercontent.com/117425577/222986304-8e28166f-7c83-4067-a0f2-11b368eb17f9.jpg)

4. Mount GPS using 3DPrinted parts. In our design, we decided to mount the GPS to the interior of the plane because it presented a cleaner solution than having it mounted onto the exterior.
   * 3D print GPS stands.
   * Melt through the top of the GPS stands so that screws can fit through.
   * Secure the 3D Printed GPS stands to the wooden rails inside the fuselage. 
   * Screw on the GPS to the stands.
   
### Wiring
* The following chart is the configuration we used on our flight controller. Refer to the [pinout for the Kakute H7 mini](https://docs.holybro.com/fpv-flight-controller/kakute-h7-mini/pinout) for more details


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





* The Heewing kit came with a BEC which came attached to two quick-release buses that power the Motors. This BEC has also been soldered onto to power our
flight controller, pdb, rear ESC/motor, and VTX.

![Wiring2](https://user-images.githubusercontent.com/117425577/220202423-3d94a367-2aad-4e95-af08-018184116720.jpg)



## PX4 Configuration
* Once PX4 has been flashed onto your flight controller you are now able to modify the parameters. This section will go over what parameters have been modified on this build and why. You can find more information on these parameters on the PX4 website [here](https://docs.px4.io/main/en/config/actuators.html)
## Actuator Outputs
1. Within the "Actuator Outputs" plane we can assign which actuator corresponds to which output on our flight controller. If you refer back to our wiring assignment you will notice that we have M1 to the front left motor or Motor 1, M2 to the front right motor or Motor 2, M3 to the rear motor or Motor 3, and so on. 
2. Assigning the PWM Minimum, and Maximum values will limit the range at which the actuators will rotate from a minimum of 800 to a maximum of 2200. It is important to have the value appropriately set to avoid your motor from tilting too far and your propeller accidentally eating into the plane.
3. Setting the disarmed values will dictate what position your motor will be at when disarmed.
### Geometry
#### MC Motors
1. Starting from the geometry section of the actuators menu, declare how many motors you have. You can modify the number of motors you want by using the "MC Motors" drop-down menu.
2. Your X position indicates how far forward or backward the motor is from the center of gravity, forward being positive and backward being negative. Conversely, the Y position indicates how far left or right the motor is in regards to the center of gravity, with left being negative and right being positive.
3. In addition, you must declare which motor is being tilted by which servo. This can be modified by the drop-down menu following the X and Y position.
4. Changing the direction CCW will change the direction the motor spins. By default, it will spin counterclockwise with respect to the FRD coordinate system around PX4FMU's Z axis or Yaw axis. For more information on the FRD coordinate system see the [PX4 Terminology page](https://docs.px4.io/main/en/contribute/notation.html)

#### Control Surfaces
* Our VTOL uses two control surfaces which include a single-channel aileron and an elevator. We are using a single channel aileron due to the limited number of signal output channels on the Kakute H7 mini flight controller.
1. Following that, the number of control surfaces can be modified by using the drop-down menu labeled "Control Surfaces".
2. If you look at the "Actuator Outputs" section you can see how the servos correspond to the flight controller's outputs.

#### Tilt Servos
1. You can alter the number of tilt servos by using the "Tilt Servos" drop-down.
2. By setting your angle at min and max tilt values you are defining the angles at which the max and min PWM values correspond to, 90 being straightforward and -90 being backward.
   * It is important to set this because you must assign the percentages where the VTOL is in multi-copter mode and transition mode which uses the max tilt and min tilt angle as the full range.

The image below shows how our VTOL's tilt parameters are assigned. When the VTOL is in multi-copter mode the value is set to 13.5% which corresponds to being completely vertical. During transition mode, it is only set to 80% because we still require vertical thrust until it fully transitions. DISCLAIMER, subject to tuning.

![VTOLTILTANGLE](https://user-images.githubusercontent.com/117425577/220211260-bbadd5ad-7194-4f5b-94d3-57c7c9989fd9.png)



The image below shows how our VTOL actuators are configured.
![Screenshot from 2023-02-19 18-05-30](https://user-images.githubusercontent.com/117425577/220202844-6ce2e315-b2d4-4f6a-8df6-d99593dc5b05.png)




