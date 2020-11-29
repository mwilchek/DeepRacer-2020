# Iteration 2 - Mixing It Up
After my first speed trial, I began to dive deeper to understand all the components that could be updated to improve the results. I also read that the re:Invent 2020 Tournament recommended to train using the "American Hills Speedway" track for that was the one it was evaluating for the wildcard tournament at the time of this project. One of the first things I did was re-deign the vehicle or agent used. I increased the overall speed and widened the increments it could speed up and slow down. I also read more how the Reward Function could be changed to maximize rewards and optimize performance for the agent to learn from. An important paper I learned from was “Implementation of the Pure Pursuit Path Tracking Algorithm” by R. Craig Coulter, 1992 from Carnegie Mellon University that discusses the concept of "Waypoints." "Waypoints" are a series of pin-pointed spots that the agent tries to follow to stay on the most optimal path of a track. With some logic of "Waypoints" added to the Reward Function, I lastly updated some of the Hyper-Parameters.

## Results
After training for about 2.5 hours this iteration, I was surprised that the evaluation results were considerably worse than my first iteration. Five attempts were made to make a complete lap, and 0 trials made it over 20% through. The best trial made through 11% of the track in 4.7 seconds before flying off again. Looking at these results, with all I added in, I figured I'd certainly do worse for my submission to the re:Invent 2020 Wildcard Tournament in Speed Trial. To my amazement, my rank increased to 332 out of 860! It took a little over 5 minutes to complete 3 laps of the track. Perhaps the evaluation metrics aren't everything when it comes to actual testing on the tournament environment.

## Steps Forward
Based on my new tournament rank, I thought of three additional things I could investigate changing. Again, update the design of the vehicle/agent, adjust the hyper-parameters more, and train for significantly longer.

## Reward Function

```python
import math


def reward_function(params):
    # Reward weights
    speed_weight = 100
    heading_weight = 100
    steering_weight = 50

    # Initialize the reward based on current speed
    max_speed_reward = 10 * 10
    min_speed_reward = 3.33 * 3.33
    abs_speed_reward = params['speed'] * params['speed']
    speed_reward = (abs_speed_reward - min_speed_reward) / (max_speed_reward - min_speed_reward) * speed_weight

    # - - - - -

    # Penalize if the car goes off track
    if not params['all_wheels_on_track']:
        return 1e-3

    # - - - - -

    # Calculate the direction of the center line based on the closest waypoints
    next_point = params['waypoints'][params['closest_waypoints'][1]]
    prev_point = params['waypoints'][params['closest_waypoints'][0]]

    # Calculate the direction in radius, arctan2(dy, dx), the result is (-pi, pi) in radians
    track_direction = math.atan2(next_point[1] - prev_point[1], next_point[0] - prev_point[0])
    # Convert to degree
    track_direction = math.degrees(track_direction)

    # Calculate the difference between the track direction and the heading direction of the car
    direction_diff = abs(track_direction - params['heading'])
    if direction_diff > 180:
        direction_diff = 360 - direction_diff

    abs_heading_reward = 1 - (direction_diff / 180.0)
    heading_reward = abs_heading_reward * heading_weight

    # - - - - -

    # Reward if steering angle is aligned with direction difference
    abs_steering_reward = 1 - (abs(params['steering_angle'] - direction_diff) / 180.0)
    steering_reward = abs_steering_reward * steering_weight

    # - - - - -

    return speed_reward + heading_reward + steering_reward
```

## Hyper-Parameters Used
|Parameter|Value|
| :---: |:---:|
|Batch Size|256|
|Entropy|0.03|
|Discount Factor|0.999|
|Loss Type|Huber|
|Learning Rate|0.00033|
|Episodes|20|
|Epochs|10|

Training time: 150 minutes