# Iteration 3 - Trial & Error
For my third iteration, I knew I didn't want to revise the reward function and just investigate updating my vehicle/agent design and the hyper-parameters. So, I went back to the garage and made a vehicle with the maximum speed of 4 m/s. I noticed that the top leaders in the tournament had vehicles reaching that speed, thus I figured I should max it out as well. Second, I maxed out the batch size that is allowed and maxed out the number of episodes used in training; which is the number of experiences or trials the agent learns from for each action taken. Essentially, I wanted to increase the amount of data used for everything the best I could. Finally, I allowed the agent to train for 12 complete hours. I hoped with a faster vehicle, more data, and way longer training I could improve my rank even more.

## Results
After training for 12 hours, I was happy to find the evaluation results were better compared to my second iteration's model. Three of the five trails, the agent made through more than 10% of the track before going off. The best trial though, the agent made it through 15% of the track and only in 7 seconds. Upon my submission to re:Invent 2020, my rank increased again to 281; beating 50 additional racers!

## Steps Forward
Now the only additional steps that I think I can only go back to the drawing board on is the Reward Function and perhaps my Learning Rate and Entropy parameters. I think adding the most data and training for a long time certainly improved the overall performance. Too bad the tournament ends in 1 day at the time of me writing this; I kind of wish I knew this existed a month ago so I could have more time to train longer models.

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
|Batch Size|512|
|Entropy|0.03|
|Discount Factor|0.999|
|Loss Type|Huber|
|Learning Rate|0.0003|
|Episodes|100|
|Epochs|10|

Training time: 720 minutes