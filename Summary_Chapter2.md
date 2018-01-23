### Intelligent Agents
##### Definitions
* **agent**: anything that can be viewed as perceiving its **environment** through **sensors** and acting upon that environment through **actuators**
* **percept**: The agent's perceptual inputs at any given instant.
* **percept sequence**: Complete history of everything the agent has ever perceived.
* **Agent Function**: Maps **percept sequence** --> **action**. Implemented by **Agent Program**

### Good Behavior: The Concept of Rationality
* Evaluate agent's suitability by examining its *consequences* on the environment.
* Desirability is captured by a **performance measure** that evaluates a sequence of environment states (*not* agent states)

##### Rationality
Rationality depends on four things
1. The performance measure that defines success.
2. The agent's prior knowledge
3. The agent's available actions
4. The agent's percept sequence

**Rational Agent**: For each possible percept sequence, a rational agent selects an action that is expected to maximize its performance measure given the evidence provided by the percept sequence and whatever built-in knowledge the agent has.

Rationality vs. omniscience: maximizing expected performance vs. maximizing actual performance.

**information gathering** collects data that modifies future precepts
**task environments** are the *problems* for which our agents are *solutions*
**PEAS**
* Performance
* Environment
* Actuators
* Sensors

##### Environment Types
* Fully vs. Partially Observable
* Single vs. multi-agent
* Competitive vs. cooperative
* Deterministic vs. Stochastic
* Episodic vs. Sequential
* Static vs. Dynamic
* Discrete vs. Continuous
* Known vs. Unknown

##### Agent Types
* Simple Reflex
    * Only use current percept
    * Condition-action rules
* Model-based Reflex
    * Use model to track parts of the world we can't see - **internal state**
* Goal-based
    * A goal helps decide between actions, choosing actions that further the goal.
* Utility-Based
    * A **utility function** is a internalized performance measure. Actions are chosen that maximize the utility function.
* Learning agents
    * Learning element - makes improvements
    * Performance element - selects external Actions
    * Critic - gives feedback on performance to learning element
    * Problem generator - encourages exploration of problem space
