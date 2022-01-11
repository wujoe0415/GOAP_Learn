###### tags: `自主學習`

# GOAP (Goal-Orientated Action Planning)
What is GOAP ?
GOAP is a simple algorithm to allow us to control the whole game AI system easier. In other words, after we have established codes about our game world as well as working logic, **we made use of a "Planner"**(code we created) to **figure out the plan for AI behavior**.
We apply two parameters(**Precondition** and **Postcondition**) on AI behavior. That is, all behaviors have their cause and effect, and we just need to control these parameters on the surface. 
## Steps
Trying to split a mission/action into fractures.
+ Without interacting
However, if you do not need dynamic interactions with other agents, you can just put them into a prosedure chain and complete every steps.

+ Interacting
Now you split them into fractures. Then you need to organize those steps with structures(precondition and effect chain).
For example :
	![](https://i.imgur.com/25NsJTf.png)
	![](https://i.imgur.com/5FgbgOM.png)

## Before coding 
+ world state 
In your craft world, what factors would influence agents behaviors?
These factors determine whether the agents can achieve the goals, and they also are asociated with preconditions

+ agent state
This is a motivation of an agent to perform deeds.
For examples:
	- beliefs : state
	- goals : need to achieve
	- actions : perform deeds
	
+ Planner factors:
After considering **world state** && **agent state**, the planner come up with a plan, which will send back to the agent, then executing it.


## Coding 
### 🖼 Serializable
On the very top of your code, you had better adding **[Serializable]** above the following code(WorldStates, Agents and Planner etc).
::: info 
**Serializable** is used for converting figures into a surface that can edit simplily.
In Unity, we call it **Inspector**.
:::
### 👑 WorldStates
+ **Dictionary<string key, int value>**
string store WorldState object
int store object amount
For example: AvailableCubicle, 3

+ WorldStates (Constructor)
	Construct a new woldstate object(dictionary) called **states**.
+ HasState
	return ContainsKey(key)
+ AddState
	states.add(string, int)
+ ModifyState
	modify states[key] value
+ RemoveState
	remove states[key]
+ SetStste
	Combine AddState and ModifyState
+ GetStates
	return states

### 🎡 GWorld
+ Queue
	Estabishing a queue of GameObject.
	- **Add[GameObject]**: Finishing actions and enqueuing it.
	- **Remove[GameObject]**: Participanting actions
	
### 🎎 GAgent
Every AI inherits GAgent code. In general, we set AI's SubGoals in each code.
+ **SubGoal**
	We set several goals enable AIs to attain.
	In A* algorithm, subgoals are more likely equal to endlines.
+ **Queue**
	We also build an action queue in GAgent code, and we use if-sentence to determine whether to execute it.
	Watch out! Queue is **FIFO structrue**, so it is consistent with A* algorithm to enqueue temperory goals in it.
	
### 🎞 GAction
Every action would inherit GAction code, and we declare the public parameters so that which can display on Inspector Windows.
+ **Precondition** && **Postcondition**
	We declare two dictionaries to store the condition, in the meantime, we check whether next actions are achieveable.
	If worldstates have contained or achieved postcondition of previous actions, we executed actions.

+ **General action**
	Every action inhereit GAction, we just need to build two functions and return boolean value for GPlanner plan.

### 🔑 GPlanner
+ **BuildGraph** 
	We use A* algorithm to find the cheapest plan.
	![](https://i.imgur.com/JTKHGlJ.png)
	
	We build nodes of temporary goals, tree of every actions. Every leaf refers to each **SubGoal**.
+ **GPlanner**
	GPlanner considers **actions of Gaction** and **usable Actions of Precondition and Postcondition** , return branch(leaves / actions) to attain SubGoals.