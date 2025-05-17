Devise three example tasks of your own that fit into the MDP framework, identifying for each its states, actions, and rewards. Make the three examples as different from each other as possible. The framework is abstract and flexible and can be applied in many different ways. Stretch its limits in some way in at least one of your examples

--------------

1. One of the choice that I have to keep making is when I pick up my son. I need some information (states) of the environment:
	- Whether it's raining
	- How many minutes until the bus come
	- Whether the bus actually come (yes, sometimes it DID NOT come), so I have to take another bus, but this means I have to wait a little longer and may miss my return bus.
	- Whether or not it's prayer (I'm muslim) time. I have to make sure I have enough time to pray before the bus or after (if it's unavoidable)
	- Whether or not my kid asked me for picking up early, or picking up late.
	  
	Some actions I need to take: bring umbrella (maybe heavy), bring his raincoat (depends on the possibility of huge rain), maybe bring his carseat if there's a possibility of returning with an uber. I also need to always keep track of the bus via an app. The app has a probability of failing on its tracking (fails completely or delayed).

I then put this in google gemini to formalize it, and this is what it's giving me

# MDP Example 1: Picking Up Son

This task involves an agent (the parent) making a series of decisions to pick up their son from a location (e.g., school, bus stop) efficiently and comfortably, considering various stochastic environmental factors.

## 1. States (S)

A state `s ∈ S` represents a complete description of the environment relevant to making a decision. The state can be defined as a tuple:

`s = (Weather_Cond, Time_to_Bus_A, Bus_A_Status, Prayer_Status, Time_to_Prayer_Deadline, Son_Request, Current_Loc, Items_Carried, App_Reliability, Current_Time)`

Where:

* **`Weather_Cond`**: Categorical (e.g., `Sunny`, `Cloudy`, `Light_Rain`, `Heavy_Rain`, `Rain_Forecasted`)
* **`Time_to_Bus_A`**: Discrete or continuous (e.g., in minutes: `15`, `5`, `0` for arrived, `-1` for unknown/delayed by app)
* **`Bus_A_Status`**: Categorical (e.g., `Expected`, `Delayed_App_Confirmed`, `Missed_Agent_Side`, `No_Show_Confirmed`)
* **`Prayer_Status`**: Categorical (e.g., `Not_Yet_Time`, `Approaching_Window`, `Window_Active`, `Window_Closing_Soon`, `Window_Missed`)
* **`Time_to_Prayer_Deadline`**: Discrete (e.g., in minutes remaining to pray within the preferred window)
* **`Son_Request`**: Categorical (e.g., `Pickup_On_Time`, `Request_Early_Pickup`, `Request_Late_Pickup`)
* **`Current_Loc`**: Categorical (e.g., `Home`, `En_Route_to_Stop1`, `At_Stop1`, `On_Bus_A`, `En_Route_to_School`)
* **`Items_Carried`**: Set/Binary flags (e.g., `{Umbrella}`, `{Umbrella, Raincoat, Carseat}`, `{}` )
* **`App_Reliability`**: Categorical, based on recent performance (e.g., `High`, `Medium`, `Low`, `Failed`)
* **`Current_Time`**: Continuous or discretized (e.g., `14:30` for 2:30 PM)

**Example State `s1`:**
`(Cloudy, 10, Expected, Approaching_Window, 30, Pickup_On_Time, Home, {}, High, 14:30)`
*(Interpretation: It's cloudy, primary bus A is expected in 10 mins, prayer window is approaching with 30 mins left, son expects on-time pickup, agent is at home with no items, app reliability is high, and it's 2:30 PM.)*

## 2. Actions (A)

Actions `a ∈ A(s)` are choices available to the agent in a given state `s`.

**Actions from `Home`:**

* `A1_Leave_For_Bus(Bus_ID, Items_to_Bring_List)`: e.g., `Leave_For_Bus(Bus_A, {Umbrella, Raincoat})`
* `A2_Wait_And_Reevaluate(Minutes)`: e.g., `Wait_And_Reevaluate(5)`
* `A3_Pray_Now`: (Available if `Prayer_Status` is `Approaching_Window` or `Window_Active`)
* `A4_Check_Bus_App`: (Updates `Time_to_Bus_A`, `Bus_A_Status`, `App_Reliability` in the next state)
* `A5_Order_RideShare(Destination)`: e.g., `Order_RideShare(School)`
* `A6_Prepare_Item(Item)`: e.g., `Prepare_Item(Carseat)` (changes `Items_Carried` part of state if still at home)

**Actions at `Bus_Stop`:**

* `B1_Board_Bus(Bus_ID)`: (If bus arrives)
* `B2_Wait_For_Bus(Bus_ID, Minutes)`: e.g., `Wait_For_Bus(Bus_A, 10)`
* `B3_Switch_To_Alternative_Bus_Stop(Stop_ID)`
* `B4_Order_RideShare(Destination)`
* `B5_Check_Bus_App`

**Actions if `Bus_A_Status` becomes `No_Show_Confirmed`:**

* `C1_Wait_For_Next_Scheduled_Bus(Bus_ID)`
* `C2_Go_To_Alternative_Route(Route_ID)`
* `C3_Order_RideShare(Destination)`
* `C4_Walk_To_Destination` (if feasible)

## 3. Rewards (R)

Rewards `R(s, a, s')` are scalar values received after transitioning from state `s` to state `s'` due to action `a`. The goal is to maximize the cumulative reward.

**Positive Rewards:**

* `+200`: Picking up son on time (matches `Son_Request` or default on-time).
* `+50`: Picking up son within an acceptable window (e.g., +/- 10 mins of target).
* `+70`: Completing prayer within its designated window.
* `+10`: Arriving at the bus stop just as the bus arrives (minimal personal wait time).
* `+5`: Successfully using the bus app to make an informed, beneficial decision.

**Negative Rewards (Costs):**

* `-5` per minute: Late for son's pickup beyond the acceptable window.
* `-2` per minute: Agent's own waiting time (e.g., at bus stop, for Uber).
* `-30`: Getting wet in rain (agent or son).
* `-50`: Son getting significantly wet/uncomfortable due to weather.
* `-100`: Missing prayer window entirely.
* `-40`: Prayer performed late/rushed due to poor planning.
* `-(Fare_Amount)`: For using Uber/Taxi (e.g., `-25` for a $25 fare).
* `-10`: Carrying unnecessary heavy items (e.g., car seat when bus is reliably taken and not needed).
* `-50`: Primary bus confirmed as a no-show (major inconvenience).
* `-5`: Bus app provides misleading information leading to a worse outcome.

**Terminal States:**

* Son successfully picked up.
* Decision to abandon pickup attempt for the day (e.g., extreme weather, multiple bus no-shows, too late). -> my comment: GEMINI, HELLO? SO I SHOULD JUST ABANDON MY SON? :cries:

## 4. Stretching the Limits: Partial Observability (POMDP Aspect)

This example naturally "stretches the limits" of a basic MDP because of **partial observability**:

* **Unreliable Bus App:** The agent doesn't always know the *true* location or arrival time of the bus. The app provides an *observation*, which might be noisy, delayed, or incorrect. The `App_Reliability` state component tries to capture this, but the true bus state is hidden.
* **Bus "No-Show" Uncertainty:** The agent doesn't know for sure if a bus is a "no-show" until a significant amount of time has passed beyond its expected arrival. Until then, it's a belief (e.g., "Bus is 5 minutes late, might still come" vs. "Bus is 20 minutes late, probably a no-show").
* **Weather Prediction vs. Reality:** A forecast is an observation, but the actual weather upon stepping out might differ.

	This means the agent often operates based on a *belief state* (a probability distribution over possible true states) rather than a perfectly known state. This makes the problem more aligned with a **Partially Observable Markov Decision Process (POMDP)**, which is an extension of MDPs designed to handle such uncertainties in state observation. The actions like `Check_Bus_App` are information-gathering actions crucial in POMDPs to update the belief state. 


simplified visualization from Claude

![[Pasted image 20250513115420.png]]