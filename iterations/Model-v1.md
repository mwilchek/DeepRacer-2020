# Iteration 1 - Accepting Default Parameters

This was my first run of DeepRacer that accepted the default Hyperpameters and using their reward function to stay inside the border of the track. I also selected the track "AWS Summit Raceway" which was created to prepare racers for the 2020 AWS DeepRacer League. It was described that the "AWS Summit Raceway" provides a solid training warm up for any agent. In addition, I only let the training run for 1 hour to learn how the rest of the console works. 

## Results
Ideally, the vehicle should be able to make 3 laps on the track without going off and in a competitive time. After training for 1 hour, the evaluation section of the console tested the best model in a final simuation. The model successfully completed 1 full lap of the track in 36 seconds! However, in lap 2 the vehicle went off the track after completeing 42% of it within 15seconds. In the third lap, the model went off the track again, but only after completeing 59% of the track within 22 seconds.

## Steps Forward
For a first time execution, I think without switching the reward function or any of the hyperparmeters I think it would have done better in laps 2 and 3 if it trained longer than 1 hour. However, for my second iteration I wanted to research more about each of the configurations and how a reward function could change.

## Reward Function

```python
    def reward_function(params):
    	"""
    	Example of rewarding the agent to stay inside the two borders of the track
    	"""

    	# Read input parameters
    	all_wheels_on_track = params['all_wheels_on_track']
    	distance_from_center = params['distance_from_center']
    	track_width = params['track_width']

    	# Give a very low reward by default
    	reward = 1e-3

    	# Give a high reward if no wheels go off the track and
    	# the agent is somewhere in between the track borders
    	if all_wheels_on_track and (0.5 * track_width - distance_from_center) >= 0.05:
        	reward = 1.0

    	# Always return a float value
    	return float(reward)
```