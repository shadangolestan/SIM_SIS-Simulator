# Introduction
This repository is the implementation of our paper published below:
https://www.mdpi.com/1424-8220/20/24/7137 

# Requirements
Unity 3D version: 2018.2.18f1


# Inputs
Our simulator, SIMSIS, requires two files as input:
1. Space Model: The space model is the specification of the indoor environemnt in IFC format. If you do not already have the IFC version of your space, we would recommend to use [Autodesk Revit](https://www.autodesk.ca/en/products/revit/overview?term=1-YEAR&tab=subscription) to develop one.

The followint steps show how to difine affordances for the space model's objects. Follow these steps based on the actions that you would like the agent to perform.

a. Run SIMSIS in Unity 3D.
b. Select BIM Editor
c. Select your IFC file (Other inputs are not important)
d. You should be able to see the digital twin of your space from top view.
e. Right click on an object to select it (for example a chair)
f. Click on Add Property. For Name textbox, write "action", and for Value textbox, write the action that the object can affort (for the chair example, it would be "Sit")
g. Once you are done, save the model and close the simulator. 
h. Copy your newly saved IFC file in Assets/IFC Models directory. The IFC model of our testbed is uploaded for reproduce the results or use it as an example. 

2. Agent Model: The agent model defines a sequence of actions that your agent should perform. The specification is stored in a JSON file; a sample excerpt describing sleeping and eating behaviour is shown below.
```
{
  "Actions": {
    "Sleep": {
      "name": "Sleep",
      "duration": 480,      
      "probability": 100,
      "occurrence": 1,      
      "requires": [],       
      "post": [],           
      "times": [            
        [ 0, 8 ]            
      ],
      "aliases": []         
                            
    },
    
     "Eat": {
      "name": "Eat",
      "duration": 30,
      "probability": 100,
      "occurrence": 3,
      "requires": [ "Cook" ],
      "post": [ "Wash" ],
      "times": [
        [ 8, 9 ],
        [ 12, 13 ],
        [ 18, 19 ]
      ],
      "aliases": []
    },
  }
}
```
Each of the actions are taylored to the affordance that we defined in the space model. The properties of the actions are explained below:

a. Duration: How long the activity will last in simulation time, indicated in minutes. A value of 480 would translate to 8 hours of simulation time. 
b. Probability: The program takes in a list of actions and randomly sorts the list every time a new action needs to be selected. The actions are then selected sequentially. This selection method ensures an equal probability of all actions being selected, which may not be desirable for agent behaviour. To remedy this, the probability we define is the chances of performing the selected action, allowing for more variation in behaviour. Probability is indicated from 0 - 100. If the action is exclusively a precondition, its probability is 0, as it will be forcibly acted upon when needed and at no other time. 
c. Occurrence: The maximum number of times this action can occur daily.
d. Requires: Actions that must be performed before this action.
e. Post: Actions that must be performed after this action.
f. Times: Time constraints on when this action can be performed. The range of times and values are indicated using a 24-hour clock. (Ex. [13, 16])
g. Aliases: Alternate names for the action the agent is taking to make them seem more lifelike. This prevents the creation of redundant actions, as "Eating lunch" and "Eating dinner" would effectively be the same thing.

Once you are done, copy the JSON file in the "Assets/Task Files" directory.

# Usage
Run SIMSIS in Unity 3D and click Simulation on simulation. It asks to input the agent and space that you would like to simulate, as well as the time that the whole simulation should take in real-world; lower number runs the simulation faster. We would recommend for a task file of a whole day, choose 10 minutes.