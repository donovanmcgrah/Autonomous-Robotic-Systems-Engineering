AURO ROS2 Package
----------------------------
There is one simulation environment ROS node, `item_manager`, and three ROS nodes per
robot `item_sensor`, `robot_sensor` and `zone_sensor`.

## Items and zones
There are three types of **items**, identified by distinct colours: red, green, and blue. Similarly,
there are, by default, four **zones**, where items can be deposited,  located in the four corners of the arena
and identified by distinct colours: cyan, purple, green, and pink.

### Picking-up and offloading an item
Items can be picked up when close by using the ROS service `/pick_up_item`. Similarly, when a robot
holds an item it may offload it by using the ROS service `/offload_item`. Details of the ROS facilities
of the simulated environment are discussed further below.

### Depositing an item in a zone
To deposit an item, a robot must **offload** the item while over a zone. When an item is successfully 
deposited, it will be automatically replaced in the arena. 

Initially a zone can accept a deposit for an item of any colour, but 
**once an item has been deposited then only items of the same colour will be subsequently accepted** afterwards. 
In particular, this means that if an item is offloaded in a zone but cannot be deposited, then the 
item will stay on the arena floor. Note that an item may be offloaded anywhere in the arena, at any time.

# Simulation environment
A valid solution to the problem can make use of multiple robots, where each robot is
identified by a unique identifier `robotX`, where `X` is a number.

## Simulation environment nodes
The functionality of the nodes and their ROS interfaces are described in detail below.

### item_manager
This node keeps track of items in the simulated arena. 

It provides the ROS services `/pick_up_item` and `/offload_item`, for picking and offloading items
in the arena.

It also publishes the topics:
* `/item_log`: provides a log of how many items of each colour have been collected, so far.
* `/item_holders`: provides a view of the which item is held by what robot.

## Robot nodes
The following nodes are executed per robot, namespaced according to the robot's identifier. For
example, the topic `items` will be available as `/robot1/items` for `robot1`.

### item_sensor
This node processes the RGB camera image data and identifies items based on their colour.

### robot_sensor
This node processes the RGB camera image data and identifies other robots based on their colour.

### zone_sensor
This node processes the RGB camera image data and identifies zones based on their colour.
