This tutorial shows you how to publish a uORB message once written to transmit data from laser sensor to see a value in ground station or mavros.  
Clarence 2015.12.22

1. Replace Folder "mavlink/include/mavlink/v1.0/common". Add "msg/laser_distance.msg" to your msg folder. Compile.

2. Replace Folder "src/modules/mavlink". Compile.

3.add the following code in your own header file:
#include <uORB/uORB.h>
#include <uORB/topics/laser_distance.h> 

orb_advert_t _laser_distance_pub;  //define a publisher

4.add the following code in your own cpp file, then compile:

_laser_distance_pub(nullptr)   //initialize your publisher. See code in src/drivers/airspeed. You'll find _airspeed_pub similar to this one. It is a common way to publish a uORB topic in Px4.

laser_distance_s distance;    //define a struct to store values. This struct has four Float32 values, named by min_distance, angle, laser_x and laser_y. 
memset(&distance, 0, sizeof(distance)); //set memory for struct distance.

distance.angle = 10.0; //set value like this

//publish the struct with ORB_ID "aser_distance", which will be sent to ground station. 
 if (_laser_distance_pub == nullptr) {
		_laser_distance_pub = orb_advertise(ORB_ID(laser_distance), &distance);
	} else {
		orb_publish(ORB_ID(laser_distance), _laser_distance_pub, &distance);
	}

5.upload this new firmware to px4 and launch mavros px4.launch. Note that Baud rate must be right and your movros must have laser_receiver topic. Then use "rostopic ehco" to see the values in topic "/mavros/laser_receiver/laser_receiver". You should see the values you've sent from px4. 
