https://www.youtube.com/watch?v=2iF9PRriA7w

![](Reinforcement%20Learning/Markov%20Decision%20Process/Pasted%20image%2020250517140150.png)

above is an example of decision making

the model is too simple

can get more complex after we weigh in the uncertainty


this is an example of a car choice, but there are three traffic levels

![[Pasted image 20250513061023.png]]



![[Pasted image 20250513061112.png]]

this process is an example of markov decision process

MDP is a states and action (just like the shortest path), expect now, when the action is taken, there's a probability distribution outcome the outcome that can be reached

markov assumption said: the probability of the outcome only depends on the current state. (first order)

we can say the 2nd order markov system that says only the current and previous that determines the probability

![[Pasted image 20250513095808.png]]

another example of a more complete markov decision process

some actions are deterministic (thick dots): bike, for example, 45 minutes without any probabilistic behaviour
some actions are stochastic: 20% to have light traffic, 70% to have medium traffic, 10% to have heavy traffic on car.

----
How do we solve MDP?

two parts of the answer: what the solutions going to look like, what's the algorithm we're going to use

for the former, if it's a shortest path problem, the solution is a "path", which is a sequence of action from start to finish.

in MDP, we can't use the "path", because when a step is taken, the world changes probabilistically, so, the world state is unknown for every step. That means, instead of a "path", we have something called a "policy".

Policy is a lookup table: when in this state, take this action. State can be simple, or can be augmented by extra information like the time of the day, how much time remaining, how much time we wait, etc.

what is the optimal action to reach a particular goal -- one of the main question.

and for MDP, one of the common one is to minimize the expected (average) cost to goal.

the way to solve this is by something called [[bellman equation]].

we can't use a* or djikstra since both deal with deterministic problems. A* is a heuristic search algorithm, and there are also heuristic algorithms for MDP.

Need to use recursive algorithm to solve this by averaging the cost (?) and then solve it recursively (still dont get it)

For the example above, if we only use average value, Car is the solution, but 1 over 10 time, we will be very late (because of high traffic).

For example, we have another objective to not be longer than 60 minutes, so we have to always take a bike (the most deterministic option). The car can take longer than 60. and train can also take longer than 60 (non-zero chance).

from the video: the most optimum strategy to get under 60 minutes, while minimizing time is to wait for 3 train cycle, and if train never arrives, go home and take the bike.

