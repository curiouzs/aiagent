# Developing AI Agent with PEAS Description
## AIM
To find the PEAS description for the given AI problem and develop an AI agent.

## THEORY
We are given with two locations: Location A and Location B which can be either clean or dirty.
The Agent has to clean the environment if its dirty and if its clean, it should switch to the other location and repeat the process.
This way we can define a robot vacuuum cleaner.

## PEAS DESCRIPTION
| Agent type    | performance  measurement      |environment  |   Actuators         |  sensors                       | 
|-------------  | ---------------------------   | ----------- |-------------------- | ------------------------------ | 
| Vaccum cleaner| Cleanliness ,Moving locations| Room locations       |  Brushes and Vaccum Extractor|  Camera,Dirt detection sensor |

## DESIGN STEPS:
### STEP 1:
Identifying the input:
The agent must take the input (percept) from the environment.
### STEP 2:
Identifying the output:
Through the given percept, the agent must perform the right action that is correct.
### STEP 3:
Developing the PEAS description:
Develop the PEAS description which is to know the sensors, performance, environment and actuators and its functionalities.
### STEP 4:
Implementing the AI agent
### STEP 5:
Measure the performance parameters

## PROGRAM
Include your agent code here
```python
#DEVELOPED BY: M.LOKESH KRISHNAA
#REGISTER NO: 212220230030
```
```python
import random
class Thing:
    def is_alive(self):
        return hasattr(self, 'alive') and self.alive

    def show_state(self):
        print("I don't know how to show_state.")

class Agent(Thing):
    def __init__(self, program=None):
        self.alive = True
        self.performance = 0
        self.program = program
        return False

def TableDrivenAgentProgram(table):
    percepts = []
    def program(percept):
        percepts.append(percept)
        action=table.get(tuple(percept))
        return action
    return program

loc_A, loc_B, loc_C, loc_D, loc_E, loc_F, loc_G, loc_H, loc_I = (0,0), (0,1), (0,2), (1,2), (1,1), (1,0), (2,0), (2,1), (2,2)


def TableDrivenVacuumAgent():
    table = {(loc_A, 'Clean'): 'Right1',
             (loc_A, 'Dirty'): 'Suck',
             (loc_B, 'Clean'): 'Right2',
             (loc_B, 'Dirty'): 'Suck',
             (loc_C, 'Clean'): 'Up1',
             (loc_C, 'Dirty'): 'Suck',
             (loc_D, 'Clean'): 'Left1',
             (loc_D, 'Dirty'): 'Suck',
             (loc_E, 'Clean'): 'Left2',
             (loc_E, 'Dirty'): 'Suck',
             (loc_F, 'Clean'): 'Up2',
             (loc_F, 'Dirty'): 'Suck',
             (loc_G, 'Clean'): 'Right3',
             (loc_G, 'Dirty'): 'Suck',
             (loc_H, 'Clean'): 'Right4',
             (loc_H, 'Dirty'): 'Suck',
             (loc_I, 'Clean'): 'Start',
             (loc_I, 'Dirty'): 'Suck',
    }
    return Agent(TableDrivenAgentProgram(table))
#right1,2,3,4 start left1,2 up1,2

class Environment:
     def __init__(self):
        self.things = []
        self.agents = []

    def percept(self, agent):
        raise NotImplementedError

    def execute_action(self, agent, action):
        raise NotImplementedError

    def default_location(self, thing):
        return None

    def is_done(self):
        return not any(agent.is_alive() for agent in self.agents)

    def step(self):
        if not self.is_done():
            actions = []
            for agent in self.agents:
                if agent.alive:
                    actions.append(agent.program(self.percept(agent)))
                else:
                    actions.append("")
            for (agent, action) in zip(self.agents, actions):
                self.execute_action(agent, action)

    def run(self, steps=1000):
        for step in range(steps):
            if self.is_done():
                return
            self.step()

    def add_thing(self, thing, location=None):
        if not isinstance(thing, Thing):
            thing = Agent(thing)
        if thing in self.things:
            print("Can't add the same thing twice")
        else:
            thing.location = location if location is not None else self.default_location(thing)
            self.things.append(thing)
            if isinstance(thing, Agent):
                thing.performance = 0
                self.agents.append(thing)

    def delete_thing(self, thing):
        try:
            self.things.remove(thing)
        except ValueError as e:
            print(e)
        if thing in self.agents:
            self.agents.remove(thing)


class TrivialVacuumEnvironment(Environment):
    """This environment has two locations, A and B. Each can be Dirty
    or Clean. The agent perceives its location and the location's
    status. This serves as an example of how to implement a simple
    Environment."""

    def __init__(self):
        super().__init__()
        self.status = {loc_A: random.choice(['Clean', 'Dirty']),
                       loc_B: random.choice(['Clean', 'Dirty']),
                       loc_C: random.choice(['Clean', 'Dirty']),
                       loc_D: random.choice(['Clean', 'Dirty']),
                       loc_E: random.choice(['Clean', 'Dirty']),
                       loc_F: random.choice(['Clean', 'Dirty']),
                       loc_G: random.choice(['Clean', 'Dirty']),
                       loc_H: random.choice(['Clean', 'Dirty']),
                       loc_I: random.choice(['Clean', 'Dirty']),}

    def thing_classes(self):
        return [ TableDrivenVacuumAgent]

    def percept(self, agent):
        return agent.location, self.status[agent.location]

    def execute_action(self, agent, action):
        if action=='Right':
            agent.location = loc_B
            agent.performance -=1
        elif action=='Left':
            agent.location = loc_A
            agent.performance -=1
        elif action=='Suck':
            if self.status[agent.location]=='Dirty':
                agent.performance+=10
            self.status[agent.location]='Clean'

    def default_location(self, thing):
        """Agents start in either location at random."""
        return random.choice([loc_A, loc_B, loc_C, loc_D, loc_E, loc_F, loc_G, loc_H, loc_I])

if __name__ == "__main__":
    agent = TableDrivenVacuumAgent()
        environment = TrivialVacuumEnvironment()
        environment.add_thing(agent)
        print(environment.status)
        environment.run()
        print(agent.performance)
```
## OUTPUT
![Untitled4 - Jupyter Notebook and 1 more page - Personal - Microsoft​ Edge 07-04-2022 00_00_57](https://user-images.githubusercontent.com/75234646/162044344-276d6bf2-58ba-42fe-864e-9193421d9e2c.png)
## RESULT
Thus an AI agent is developed and the Peas description is mentioned.
