
![[Pasted image 20250511152413.png]]


add some constraints:

- action executes -> 0.8 probability
- move at right acute angle -> 0.1 and 0.1 probability each
	- for example, move UP have 0.8 chance, move RIGHT (valid) and LEFT (wall) have 0.1 probability each

Quiz: what is the reliability of the sequence? (mean: probability of succeeding)


answer:

the probability of succeeding is equal to probability of the whole sequence -> 0.8^5, plus the small probability of going the other way around -> 0.8 x 0.1^4. The total is 0.32776.

the 0.8^5 is trivial, but it doesn't stop there.

Remember, the sequence is determined to be U,U,R,R,R. So, can the robot accidentally go to the goal with this sequence by not following the intended move?

it can!

1. U -> goes R instead (0.1)
2. U -> goes R instead (0.1)
3. R -> goes U instead (0.1)
4. R -> goes U instead (0.1)
5. R -> correct (0.8)

so, the answer is

0.8^5 + 0.1^4 x 0.8 = 0.32776