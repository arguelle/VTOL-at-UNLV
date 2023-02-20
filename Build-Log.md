# Heewing T01 Ranger VTOL (Kakute H7 mini) Build Log
The Heewing T01 Ranger is a twin motor RC plane that comes as plug and play kit out of the box. The following document 
goes over the additional steps required to convert the standard plane into a fully functioning VTOL.
## Key information
The Heewing VTOL kit does come with all of the required materials needed to have a sufficient VTOL (minus a flight controller and reciever), however, this
build log goes over all of the materials we are using in our current build which includes alternative motors and ESC's.


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
* [Mateksys PDB](https://www.getfpv.com/mateksys-servo-pdb-w-bec-5-5-36v-to-5-8-2v-svpdb-8s.html)
* FRSky Archer RS reciever
* MRO Telemetry
* MRO GPS
* Pico Blade Connectors
* JSTGH Connectors
### Tools needed
The following tools were used in this assembly
* Soldering iron
* Wire crimpers
* Size 1 screwdriver
* Alan wrench

## Assembly
1. Start by assembling the Heewing T01 Ranger
   * Out of box the plane does have some assembly required including installing the tail, wings, and screwing in adapter plates.
The [assembly guide](https://cdn.shopifycdn.net/s/files/1/0553/6573/0348/files/T1_PNP_Assembly_Guide.pdf?v=1640164559) released by Heewing
should be used to get throught the initial setup of the plane. Additionally, the VTOL conversion kit would require installing new higher powered motors,
a rear motor, and tilt servos. This buildlog does go over it however Heewing has released instructional videos on how to accomplish this.
2. Remove both motors and motor mounts and desolder the existing ESCs to replace with your new ESCs.
3. Solder the new motors onto the new ESCs and install the tilt servos.
5. Slide rear motor mount through carbon tail boom and push the motor wires through the hole on the lower end of the pipe.
6. Solder bullet connectors onto the rear motor wires poking through the end of the tail boom (optionally you can solder the wires directly to the ESC).
7. Solder bullet connectors onto the ESC for the rear motor.
8. Slide the tail boom onto the fueselauge of the plane and secure using the red nut.


### Wiring
* The following chart is the configuration we used on our flight controller.


| Port | Connection       |
|-----:|------------------|
|    M1|Front Left Motor  |
|    M2|Front Right Motor |
|    M3|Rear Motor        |
|    M5|Elevator          |
|    M6|Attatched Ailerons|
|    M7|Left Tilt Servo   |
|    M8|Right Tilt Servo  |
| UARTX|Telemetry         |
| UARTX|Reciever          |
|   I2C|GPS               |

* The image below shows a suitable configuration for wiring. From the Flight controller there are wires going from the I2C into the GPS, from UART 1 to the 
telemetry, from the RC to the Reciever, and connecting M5 - M8 to a PDB.
![IMG_5527](https://user-images.githubusercontent.com/117425577/219988439-aa2120e9-12dd-4a75-89a7-9e9a51257035.jpg)





* The Heewing kit came with a BEC which came attatched to two quick release busses that power the Motors. This BEC is has also been soldered onto in order to power our
flight controller, pdb, and rear ESC/motor.

![IMG_5532](https://user-images.githubusercontent.com/117425577/219991391-b7b10dbd-c117-4c81-9a4f-17a2dfb4e197.jpg)


## PX4 Configuration

## Tuning

## Acknowledgements
