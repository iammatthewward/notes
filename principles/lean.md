# LEAN
Lean is a philosophy used in software development that follows key principles. It originates from lean manufacturing, developed by Toyota, and has been translated to a software development methodology.

Traditionally we work in a push system. Lean uses a pull system:
#### Pull system
* There is a maximum amount of items that can be WIP for each block in the chain.
* Each block pulls it's work from the previous block when ready.
* This highlights where there is waste. If workers are left waiting for long periods, it signals an area that can be refined.

## Principles of LEAN
### Eliminate waste
 * Any activity that does not directly add value to your consumer is waste.
 * Waste can include partially completed work, extra processes, and waiting for other activities, teams, and processes.
 * In order to identify waste, decide if an activity could be bypassed or the same result achieved without it.

### Build quality in
* Take steps to avoid quality issues in code.
* This can be achieved through pair programming, TDD, constant feedback, and frequent integration.
* Understand the customers problem and solve it at the same time.

### Create knowledge
* Instead of adding more documentation or detailed planning, create knowledge by writing code.
* Use short iteration cycles to speed up the learning process, creating more feedback to learn from.
* Take specific actions to ensure you create and share knowledge within a team (code reviews, knowledge sharing sessions, training).

### Defer commitment
* Decide as late as possible, using an iterative approach to avoid making decisions earlier than they are needed.
* Deferring decisions makes it more likely that you will have been able to explore a problem more deeply before deciding on a solution.
* Commitment should be defer most on irreversible decisions, or those that will require a lot of work to undo.

### Deliver as fast as possible
* The sooner a product is delivered, the sooner feedback can be gathered and that feedback can be acted upon.
* Delivering fast reduces instances of over-engineered solutions.
* Minimising the time between a requirement being stated and the solution completed allows for less chances for the requirement to change (in reponse to changes in the market etc.).

### Respect people, empower the team
* Everyone in the team should receive the same amount of respect regardless of their role.
* Find good people and let them do their job rather than dictating to them how to do their job.
* Allow all members of a team to input into decisions.

### Optimise the whole
* If team A is working at 100% capacity, it is irrelevant if the all teams are not at 100%.
* If one team works at a higher capacity than the other, it will create inventory - which should be eliminated.
* Do not just focus on optimising individual processes or teams, optimise the whole value stream.
