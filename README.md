# AWS DeepRacer Student League 2023 🏁

This repo will hold all of my upcoming models for AWS DeepRacer Student League 2023.

I decided to check DeepRacer out of curiosity and I'm actually developing a liking to it.

Please feel free to check out my models and reach out, I'm open for improvement!

## AWS DeepRacer Student Pre-season 2023 🤯

<img src="https://user-images.githubusercontent.com/100843256/222236011-806e8d77-6279-49dd-89d1-e511c7328464.png" width="270">

- Overall Rank: **1755/5610**
- Total lap time: **02:58:922**
- Off track: -
- Completed **3** laps.


### Model 👨‍💻:

- Name of model: ***Fast-as-Heck-V2***

- Reward function code:

```python
import math

def reward_function(params):
    
    track_width = params['track_width']
    distance_from_center = params['distance_from_center']
    steering = abs(params['steering_angle'])
    direction_stearing=params['steering_angle']
    speed = params['speed']
    steps = params['steps']
    progress = params['progress']
    
    all_wheels_on_track = params['all_wheels_on_track']
    ABS_STEERING_THRESHOLD = 15
    SPEED_THRESHOLD = 5
    TOTAL_NUM_STEPS = 85
    # Read input variables
    waypoints = params['waypoints']
    closest_waypoints = params['closest_waypoints']
    heading = params['heading']
    reward = 1.0
    
    if(all_wheels_on_track == True):
        reward *= 5.0
        
    else:
        reward -= 5.0
    if progress == 100:
        reward += 100

    if not all_wheels_on_track:
        # Penalize if the car goes off track
        reward -= 1e-3
    elif speed < SPEED_THRESHOLD:
        # Penalize if the car goes too slow
        reward += 2.20 
    else:
        # High reward if the car stays on track and goes fast
        reward *= 3.0

    # Calculate the direction of the center line based on the closest waypoints
    next_point = waypoints[closest_waypoints[1]]
    prev_point = waypoints[closest_waypoints[0]]
    # Calculate the direction in radius, arctan2(dy, dx), the result is (-pi, pi) in radians
    track_direction = math.atan2(next_point[1] - prev_point[1], next_point[0] - prev_point[0])
    # Convert to degree
    track_direction = math.degrees(track_direction)
    # Calculate the difference between the track direction and the heading direction of the car
    direction_difference = abs(track_direction - heading)
    # Penalize the reward if the difference is too large
    DIRECTION_THRESHOLD = 10.0
    vid=1
    if direction_difference > DIRECTION_THRESHOLD:
        vid=1-(direction_difference/50)
    if vid<0 or vid>1:
        vid = 0
        reward *= vid
    return float(reward)

```
### Time Trial Video 🎥:

https://user-images.githubusercontent.com/100843256/222234665-0d931e52-5118-4696-8841-4e4fd6393e0c.mp4
