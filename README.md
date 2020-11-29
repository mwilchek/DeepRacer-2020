# AWS DeepRacer for re:Invent 2020

This is an informal log of my exploration of [AWS DeepRacer](https://aws.amazon.com/deepracer/) training.  My best iteration was submitted for the AWS 2020 re:Invent DeepRacer Competition. All of the work was also submitted as a final project for a graduate Cloud Computing course for the M.S. Data Science program at George Washington University.

## Reward Functions

|Iteration|Model Codename|Strategy| 
| :---: |:---:|:-----|
|1|["Model-v1"](./iterations/Model-v1.md)|Iteration 1 - Accepting Default Parameters|
|2|["Model-v2"](./iterations/v2-RacingLine.md)|Iteration 2 - Mixing It Up|

## Hyperparameter Optimizaiton
Below is a description for each Hyperparameter that can be tuned in the DeepRacer Console:
- <b>Batch Size:</b> As the agent goes around the track it collects images. The batch size is the number of experience or images that will be incorporated for each training step. The larger the size the more stable training will be. Default size is 64.
- <b>Epochs:</b> The number of times to go through the training data and update the weights/values of the model. Default epochs set is 3.
- <b>Learning Rate:</b> Size of updates the model makes during each training cycle. This parameter must be edited carefully since too large of a number may prevent convergence and too small may result in getting stuck at the local minima. Default rate is 0.0003.
- <b>Entropy:</b> As the agent drives around the track, entropy decides how many random actions the agent may take. Needed to allow the agent to explore the space or environment it's in. Ideally good to have a larger entropy in the beginning, then decrease it later so the agent learns from previous actions better. Default value is 0.01.
- <b>Discount Factor:</b> Number of steps the agent should look ahead when it's trying to make a decision through a training cycle. This factors into the action the agent takes at any current time. Example the setting of 0.999 means the agent will look ahead 1000 future steps. Default value is 0.999.
- <b>Loss Type:</b> Used to evaluate the prediction results vs. ground truth. It's what the agent must optimize to update the weights of the model and ultimately improve prediction what next action the agent should take. Default value is Huber loss.
- <b>Number of Episodes between Policy Updates:</b> Number of episodes should the agent run in between model updates. The more episodes or laps around the track, the more experience data available for the model during training. Default value is 20.
