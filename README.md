# Dancesport-Scorer

## What does the system model

The system models DanceSport competitions. Each **competition** consists of a set of events, where each **event** consists of one or more dances.
Each **dance** consist of a series of heats, where a **heat** is where a set of couples take to the dance floor to compete against one another. The heats are organised into rounds. A **round** is a set of heats, where each couple still competing will dance, once only, each of the dances in the event. 

For example if there are sixteen couples, there may be two heats in the first round, each consisting of eight couples. Let's say that from each heat, three couples get through to the next round, which is final round. So we have three heats, two in the first round and one in the second (and final) round.

OK. But that's just for dance one. If there are two dances in event then there will be separate heats for each dance. In each round, couples get judged on all of the heats that they have danced in that round. 

**More terms**
 
A dance **couple** consists of a two dancers: a **leader** and a **follower**.

### Scoring

Each event has a **panel** of **judges**. The set of judges for each round is fixed but the set of judges for different rounds may differ (and usually does). There is always an odd number of judges in a panel. Five is the most common number.

In all rounds but the final round, each judge gives each couple a mark for each heat. A **mark** is either *go through* or *go out*. Typically each judge will only award a *go through* mark to half of the couples in the set.

In the final round, each judge, for each heat, gives each couple a rank. A **rank** is a number, from 1 to N, where N is the number of couples in the final round. Note that 1 is the highest rank.

### Going though to the next round

For each couple, their marks, for each dance in the event, are summed (i.e. one point for each *go through* mark). 

To *go through*, a couple must gain a total mark that equals or exceeds the **mark threshold** for that round. The mark threshold is typically calculated as follows:

>   ceiling(number of judges / 2) * number of dances in event

Where a set of judges behave abnormally, using the **default threshold** may cause too many (or not enough) couples, to go through. In these circumstances the *head judge* may set an **alternative threshold**.

### Placement in the final round

#### Placement for each dance...

For each couple...
    * Count the number of judges ranking them in first place.
    * Count the number of judges ranking them in first or second place.
    * Count the number of judges ranking them in first, second or third place.
    * etc

Given the above counts, the following rules are app;lied to determine the position of each couple. Note that, when applying the rules, once a couple has been allocated a position (nb. allocations are made in strict order - i.e. first, second, etc.) then their counts are no longer considered when reapplying rules for subsequent positions.

**Rule 1: The couple ranked at a position (first, second, etc.) is the couple that has the majority *vote* for that position.**

That is:

* to be ranked 'first' (by this rule) a couple must have the majority of the 'first' votes;
* to be ranked 'second', a couple must have the majority of the 'first ' or 'second' marks;
* etc..

**Rule 2: If more than one couple has a majority, the couple with the greater majority gets the first available position.**

Note that there may be more than one position available - e.g consider the case where there are five judges and couple 51 is awarded two *first*s and two *second*s and couple 52 is awarded two *first*s and one *second*. Couple 51 are awarded *first place* (by rule 2); couple 52 are awarded *second place* (by re-applying rule 1). 

**Rule 3a: If two (or more) couples have the same majority, then the couple with the lower rank total gets the next available position.**

Example: 

* Couple 52 has two *first*s and one *second*... 
* ...but Couple 53 has one *first* and two *second*s.
* So each has a *majority* of three 'first or second' *votes*.
* Adding the ranks (for *first* and *second* votes only), Couple 52 have a total rank of 1+1+2=4, whereas for Couple 53 the total is 1+2+2=5, so Couple 52 get second place.

**Rule 3b: If in 3a, two (or more) couples have the same rank total, then for those couples, the votes for the next position are added to the rank totals. The couple with the lower rank total gets the next available position**

If there is still a tie then the next position is included in the rank totals, and so one.

If all votes for all positions have been counted and still there is a tie then the tied couples each get allocated the average position for the tie. For example,  if - unimaginably - four couples tied for second, third, fourth and fifth places, each would be allocated a position of "three and a half". 

**Rule 4: If no-one has a majority at any given level, include the votes for the next level and repeat.**

#### Placement for the event

The positions for each   

# List of simple things in the application

* Competition
* Couple
* Dance
* Dancer
* Event
* Heat
* Round
* Judge
* Mark
* JudgedPosition (see complicated things - below)
* Panel

# Relationships between things

* A competition is a set of events.
* An event has a set of dances.
* An event consists of a set of rounds.
* Each dance has a set of heats.
* Each heat belongs to a particular round.
* A couple consists of two dancers (a *Leader* and *Follower*).
* Couples enter events.
* Judges are in panels.
* A 'panel of judges' judges each round.
* Judges award marks to couples, in non-final heats.
* Judges award positions (ranks) to couples in final heats.
* Couples acheive final positions in final heats.
* Couples acheive final positions in events.

# List of complicated things in the application

**JudgedPosition**
The ranking given by a judge to a couple (for a dance).  
 
**Dance position**
A couple's final position, for a dance.

**EventPosition**
A couple's final position, for an event.

# List of things the application will do

* Register as a user (later)
* Add a  competition
* Select a competition.
* Delete a competition (later)

Within a competition...

* Add an event.
* Select an event.
* ...

Within an event...
* Add a dance
* Remove a dance
* Select a dance

* Add a dancer
* Remove a dancer
* Edit a dancer

* Add a couple
    * Select a dancer

* Register couple with competition
* Enter a couple into an event
* List couples in an event

* Create 1st round heats for an event
* List heats
* Select a heat
* Enter marks for a heat. (list couples in number order) 