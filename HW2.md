## Homework 2: Intelligent Agents
###### Chris Bollinger

2.2
(a) Show that a simple vacuum cleaner agent described in Figure 2.3 is indeed rational under the assumption listed on page 38.

The assumptions on page 38 state that the robot's performance is evaluated by awarding a point for each clean square each timestep, and the robot can track its position and the dirty/clean state of its square. Given these assumptions, we must show the agent is rational, with the best performance. To start, we can describe the agents behavior at a high level as "clean if there is dirt in my square, otherwise travel to the other square". To prove this agent is rational, we can consider the relative performance of an agent that chooses differently than the agent from figure 2.3. An agent function that is different must either not clean the current square when it is dirty, or not travel to the other square when the current one is clean. In the first case, an agent that does not immediately clean dirt when detected will have a lower performance, because the evaluation is based on the number of clean squares each timestep. A delay of even one timestep will reduce performance, so any agent that does not choose the "SUCK" action when the current square is dirty will be less rational that the agent in figure 2.3. In the other case, if an agent is currently in a clean square, and does not choose to move to the other square, it may be delaying the discovery of a dirty square. For the same reasons as the first case, this will result in a lower overall performance due to increasing the time until both squares are cleaned. Therefore, the agent in Figure 2.3 is rational, because any agent which chooses a different action in a given situation will at best acheive the same performance, and may do worse.

(b) Describe a rational agent function for the case in which each movement costs one point. Does the corresponding agent program require internal state?

If the agent is penalized for movement, then the agent in Figure 2.3 needs to be modified so that it does not move once both squares are cleaned. First, we can define a new state "OFF", so that the agent can choose to do nothing if all rooms are cleaned. Then, we can define the agent function to be the same as the previous agent, with the following modification: all percept histories that contain both [A, CLEAN] and [B, CLEAN] generate the "OFF" action. To implement this in an agent program requires memory to track the dirty/clean states of rooms A and B. Specifically, 2 bits of memory could capture the state of the two rooms and allow for an implementation of this agent function.

(c) Discuss possible agent designs for the cases in which clean squares can become dirty and the geography of the environment is unknown. Does it make sense for the agent to learn from its experience in these cases? If so, what should it learn? If not why not?

If squares can turn dirty, then a naive approach might be to constantly travel around the environment cleaning squares. However, if there was some particular square that had a high chance of turning dirty (a hallway or high traffic area), it would be beneficial to identify this information and respond accordingly. An agent could do this by keeping track of how often the square ends up dirty after it was cleaned, as a function of how many timesteps the measurements were from each other. In this way, the agent could learn the frequency with which each room seems to become dirty, and use this information to plan out its path through the environment, prioritizing rooms that have likely become dirty since the last time it drove through them. For the case of an unknown environment, it can be very beneficial to build a map as the agent explores, in order to clean more efficiently in the future. As a real world example, more modern Roombas and Neatos use SLAM to map houses as they are cleaned. Doing this reduces the time required to clean the house in the future, allows for more granular cleaning ("Clean the kitchen, but not the dining room"), and allows for more complete coverage. In both of these cases, the downside of learning is the memory required. Early robot vacuum cleaners avoided tracking these variables in order to stay simple and cost effective, so sometimes adding agent complexity to track all these variables may be the wrong choice depending on your use case.

2.4. For each of the following activities, characterize the environment in terms of the properties listed in section 2.3.2.
* Playing the game of Go
The game of Go (assuming no time limits) is Fully Observable, MultiAgent, Deterministic, Sequential, Static, and Discrete
* Solving Sudoku
Sudoku is Fully Observable, Single Agent, Deterministic, Sequential (assuming a move is placing a single number, otherwise would be episodic if the output of each "episode" is the solution to a given puzzle), and Discrete
* Shopping for used books on the Internet
Assuming that shopping for used books online involves checking multiple sites and comparing prices, The task is Partially Observable, Single Agent, Stochastic, Sequential, Dynamic, and Discrete
* Solving jigsaw puzzle
Solving a jigsaw puzzle is Partially Observable (not all pieces may be visible at every timestep), Single Agent, Stochastic, Sequential, Static (if no timer), and Continuous (pieces may be rotated, slid, etc.)
* Assembling furniture
Furniture assembly is Fully Observable, Single Agent, Stochastic, Sequential, Dynamic, and Continuous
* Scheduling a meeting
Furniture assembly is Fully Observable, Multi Agent, Deterministic, Episodic, Static, and Continuous (though practically speaking meeting scheduling is usually discrete on 15 minute blocks)
* Booking an airline ticket over the internet
Booking a ticket online is Partially Observable, Multi Agent (many people getting tickets on same flight), Deterministic, Sequential, Dynamic, and Discrete
* Bidding on an item in an auction
An auction is Partially Observable, Multi Agent, Deterministic, Sequential, Dynamic, and Continuous

2.6.
(a) Can there be more than one agent program that implements a given agent function? Give an example or show why one is not possible.
Yes. From the textbook, an agent program is the concrete implementation of an agent function, running within a physical system. Therefore, we could develop a robot that can run on multiple programming languages (say, C and Python), and implement the same agent function in both languages. While there is only one agent function, two distinct agent programs both implement it. As another example, some tasks (agent functions) can be completed in different ways. If we have an agent function that reverses a list given to it, this could be done by reversing the list front-to-back or back-to-front, but either program would successfully implement the agent function.

(b) Are there agent functions that cannot be implemented by agent programs?
An agent function maps any percept history to an action, but in some cases this requires infinite space. One example would be the halting problem. An agent can be described that would provide an answer on whether a problem halts or not, but this is not implementable by an agent program, since the halting problem is undecidable.

(c) Given a fixed architecture, does each agent program implement exactly one agent function?
Yes. An agent program will respond a certain way for each percept history, implementing an agent function. Given that the architecture is fixed, the program can only implement one agent function at a time. If a second function was implemented by the program, it would no longer provide the expected output of the first agent function for some percept histories.

(d) Given an architecture of n bits of storage, how many different agent programs are there?
N bits of storage can be configured into $2^N$ different combinations. In theory each one of these represents a potential agent program, although most of them will probably be useless.

(e) Suppose we keep the agent program fixed and speedup the machine by a factor of 2. Does that change the agent function?
Increasing the program speed does not change the relationship between percept histories and actions. That being said, in a dynamic environment the observed behavior of the agent may be very different when running at a faster speed, even though the agent function is exactly the same. This is because in a dynamic environment the agent running twice as fast will have a longer percept history, and so although the function is the same there will be different actions activating at a given clock time.

2.13
(a) Murphy's law: 25% of the time, the Suck action fails to clean the floor if it is dirty and deposits dirt onto the floor if it is clean. How is your agent program affected if the dirt sensor gives the wrong answer 10% of the time?
In the case where the SUCK action can deposit dirt 25% of the time instead of cleaning, it is important to never choose the SUCK action on a clean square. With the addition of an inaccurate sensor, there is now a $0.25 * 0.10 = 2.5\%$ chance that the robot deposits dirt where there was none before, a 10% chance that SUCK is not called when it should be, and a 25% chance that when SUCK is chosen while in a dirty square, it fails to clean the square. In the simple 2 room vacuum world, the problem of randomness in the SUCK action can be mitigated by moving back and forth checking rooms repeatedly to make sure they are clean. If movement has a cost, the agent should have memory and track the probability that a square it thinks is clean is actually dirty, based on repeated observations. Since the squares cannot get dirty on their own, if the agent averages its sensor readings over time, it will be able to detect a sensor misread and ignore it.

(b) Small Children: At each time step, each clean square has a 10% chance of becoming dirty. Can you come up with a rational agent design for this case?
For this case, a rational agent would need memory to track the state of all squares in the environment. Once a square has been visited and cleaned, the probability that it is still clean is $0.9^k$, where k is the number of timesteps since the square was last visited. Assuming that movement has a cost, the agent can use this information to choose at which point the likelihood of a square being dirty makes the cost of visiting it worthwhile. If we assume the basic 2 square vacuum world, with a cost of 1 for moving and a reward equal to the number of clean squares per timestep, then the agent should move on the timestep when the probability of the square being dirty is higher than 50%, which is on the 7'th timestep. At that point, the cost of traveling is outweighed by the expected loss of 1 point per timestep from the dirty room.
