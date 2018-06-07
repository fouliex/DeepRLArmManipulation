
# Deep RL Arm Manipulation

This project is based on the Nvidia open source project "jetson-reinforcement" developed by [Dustin Franklin](https://github.com/dusty-nv).
The goal of the project is to create a DQN agent and define reward functions to teach a robotic arm to carry out two primary objectives:

1. Have any part of the robot arm touch the object of interest, with at least a 90% accuracy.
2. Have only the gripper base of the robot arm touch the object, with at least a 80% accuracy.

## Building from Source (Nvidia Jetson TX2)

Run the following commands from terminal to build the project from source:

``` bash
$ sudo apt-get install cmake
$ git clone https://github.com/fouliex/DeepRLArmManipulation.git
$ cd DeepRLArmManipulation
$ git submodule update --init
$ mkdir build
$ cd build
$ cmake ../
$ make
```

During the `cmake` step, Torch will be installed so it can take awhile. It will download packages and ask you for your `sudo` password during the install.

## Reward Functions

### The Gazebo Arm Plugin 
The robotic arm model found in the Gazebo world calls upon a gazebo plugin called `ArmPlugin`. This plugin is responsible
 for creating the Deep Q-Network(DQN) agent and training it to learn to touch the prop.
 
 The gazebo plugin shared libgazeboArmPlugin.so a object file that attached to the robot model in the Gazebo world. That
 object file is responsible for integrating the simulation environment with the Reinforcement Learning(RL) agent. The  plugin is defined
 int the `ArmPlugin.cpp` file located in the [gazebo](./gazebo) folder.

### The Arm Plugin Source Code
The `ArmPlugin.cpp` take advantage of the C++ API. This plugin creates specific functions for the class ArmPlugin defined in
`ArmPlugin.h`.

####### ArmPlugin::Load()
This function is responsible for creating and initializing nodes that subscribe to two specific topics, one for the camera
and one for the contact sensor for the object.

###### ArmPlugin::onCameraMsg()
This function is the calllback function for the camera subscriber. It takes the message from the camera topic, extracts
the image and saves it. This is then passed to the DQN.

###### ArmPlugin::onCollisionMsg()
This function is the callback function for the object's contact sensor. It is used to test whether the contact sensor,
 called `my_contact`, defined for the obect in Gazebo worl, observes a collision with another element/model or not.
 
###### ArmPlugin:createAgent()
This function serves to create and initialize the agent.Various parameters that are passed to the `Create()` function for
the agen are defined at the top of the file such as:

```cpp
#define INPUT_WIDTH   512
#define INPUT_HEIGHT  512
#define OPTIMIZER "None"
#define LEARNING_RATE 0.0f
#define REPLAY_MEMORY 10000
#define BATCH_SIZE 8
#define USE_LSTM false
#define LSTM_SIZE 32
```
###### ArmPlugin:updateAgent()
## Hyperparameters

## Result

## Future Work




## Project Environment

To get started with the project environment, run the following:

``` bash
$ cd RoboND-DeepRL-Project/build/aarch64/bin
$ chmod u+x gazebo-arm.sh
$ ./gazebo-arm.sh
```

<img src="https://github.com/dusty-nv/jetson-reinforcement/raw/master/docs/images/gazebo_arm.jpg">

The plugins which hook the learning into the simulation are located in the `gazebo/` directory of the repo. The RL agent and the reward functions are to be defined in [`ArmPlugin.cpp`](gazebo/ArmPlugin.cpp).
