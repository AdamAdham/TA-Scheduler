German University in Cairo Department of Computer Science Dr. Nourhan Ehab

Concepts of Programming Languages, Spring Term 2022

Project 1: Labs Scheduling System

Due: 6th May 2023

1. Project Description. In this project you are going to implement a scheduling system in Prolog to assign the weekly labs of a course to the available TAs. The following should be noted:

-Each lab should be assigned exactly one TA.

-Each TA has a teaching load (the number of labs they are required to teach per week).

-Each TA should not be assigned more labs than their teaching load (but can get less load).

-The number of slots assigned to each TA per day must not exceed a particular specified number.

2. Required Predicates. Your implementation must contain the five below predicates. Read the description of all of them before you start your implementation.

Note that:

1. You can add any other helper predicates you need.
1. You can use any predefined predicates except for assert and retract.
1. It is easier to implement the predicates in the reverse order of how they are listed below.
1) week\_schedule(WeekSlots,TAs,DayMax,WeekSched) such that:
- WeekSlots is a list of 6 lists with each list representing a working day from Saturday till Thursday. A list representing a day is composed of 5 numbers representing the 5 slots in the day. The number at position i in a day list re- presents the number of parallel labs at slot i.

Example:
``` Prolog
WeekSlots = [ [0, 0, 0, 0, 0], [2, 1, 2, 3, 0],[2, 0, 1, 2, 0] ,
[0, 1, 1, 0, 0] , [1, 0, 0, 2, 2] , [2, 1, 3, 1, 0] ]
```
The first list represents that Saturday has no scheduled labs. The second list represents that Sunday has 2 labs in the first slot, a lab in the second slot, 2 labs in the third slot, 3 labs in the fourth slot, and no labs in the fifth slot. And so on ...

- TAsis a list of structures of the form ta(Name,Load) where Nameis the name of the TA and Load is an integer representing their teaching load.

Example:
``` Prolog
TAs = [ta(y, 4), ta(h, 7), ta(r, 8), ta(s, 8)]
```
This means that the course has four teaching assistants namely; y, h, r, and s. y should teach 4 labs a week, h should teach 7 slots a week, r should teach 8 slots a week, and s should teach 8 slots a week.

- DayMaxis the maximum number of labs a TA can be assigned per day.

Example:
``` Prolog
DayMax = 3
```
- WeekSched is the weekly assignment of TAs to the labs. It is represented as a list of 6 lists. Each list represents a working day from Saturday to Thursday. Position i in a day list is a list containing the names of the assigned TAs to slot i in the day.

week\_schedule/4 succeeds if WeekSchedis a possible assignment of the labs to the teaching assistants in TAs according to WeekSlots so that none is assigned more than their teaching load or assigned more than DayMaxlabs per day.

Example Query:

Assume that WeekSlots, TAs, and DayMaxare substituted in the below query with the values given in the examples above. WeekSchedis the only variable in the below query.

``` Prolog
week_schedule(WeekSlots,TAs,DayMax,WeekSched).

WeekSched = [ [[], [], [], [], []],

[[r, y], [y], [r, h], [r, h, y], []], [[h, y], [] , [h] , [h, s] , []], [[], [s], [s], [], []],

[[s], [], [], [s, r], [s, r]],

[[h, r], [s], [r, h, s], [r], []]

]
```
Since there are no labs on Saturday, no TAs are assigned on Saturday. Since there

are two labs on Sunday first slot, the first slot is assigned two TAs r and y. The second slot is assigned only y since it has only 1 lab, and so on. The schedule does not assign any of the TAs more than their teaching load, and does not assign anyone more than 3 labs a day.

2) day\_schedule(DaySlots,TAs,RemTAs,Assignment) such that:
- DaySlots is a list of 5 numbers representing the number of parallel labs in the 5 slots of the day.
- TAsand RemTAsare lists of TA structures.
- Assignment is a list of lists of TA names in TAsrepresenting the assignment of the day.

day\_schedule/4 succeeds if Assignment is a possible day assignment given the available DaySlots and list of course TAs, while RemTAsis the list of updated TA

structures after the day assignment.

Example Query:
``` Prolog
day_schedule([2, 1, 2, 3, 0], [ta(y, 4), ta(h, 7), ta(r, 8), ta(s, 8)],
RemTAs,Assignment).

RemTAs = [ta(y, 1), ta(h, 5), ta(r, 5), ta(s, 8)] Assignment = [[r, y], [y], [r, h], [r, h, y], []]
```
Since y was assigned 3 lab in the day, y’s load is decremented by 3. Similarly, h’s load is decremented by 2, and r’s load decremented by 3. s’s load is not decremen- ted since s was not assigned any slots in this day.

3) max\_slots\_per\_day(DaySched,Max) such that:
- DaySched is a day schedule showing the assignment of the TAs in every slot.
- Maxis a number showing the maximum amount of labs a TA can be assigned in a day.

max\_slots\_per\_day/2 succeeds if no TA is assigned more than Maxlabs in DaySched. 

Example Query:
``` Prolog
max_slots_per_day([[y, h], [y], [r, s], [r, s, h], []], 1). false

max_slots_per_day([[y, h], [y], [r, s], [r, s, h], []], 3). true
```
4) slot\_assignment(LabsNum,TAs,RemTAs,Assignment) such that:
- LabsNumis a number representing the amount of parallel labs in this slot.
- TAsis a list of TAs structures.
- RemTAsis the updated list of TAs structures after the assignment to this slot.
- Assignment is a list of the names of TAs in TAsassigned to this slot.

slot\_assignment/4 succeeds if Assignment is a possible assignment to a single slot with LabsNumlabs and RemTAsis the list of modified TAs after the assignment.

Example Query:
``` Prolog
slot_assignment(3, [ta(y, 4), ta(h, 7), ta(r, 8), ta(s, 8)],

RemTAs,Assignment).

RemTAs = [ta(s, 7), ta(y, 3), ta(h, 6), ta(r, 8)] Assignment = [s, y, h]
```
Since s, y, and h are assigned to this slot, their remaining teaching loads are de- cremented in RemTAs.

5) ta\_slot\_assignment(TAs,RemTAs,Name) such that:
- TAsand RemTAsare lists of TA structures
- Nameis a name of a TA in TAs.

ta\_slot\_assignment/3 succeeds if RemTAsis the list of TA structures resulting from updating the load of TA Namein TAs.

Example Query:
``` Prolog
ta_slot_assignment([ta(y, 4), ta(h, 7), ta(r, 8), ta(s, 8)],RemTAs,y). RemTAs = [ta(y, 3), ta(h, 7), ta(r, 8), ta(s, 8)]
```
3. Teams. You are allowed to work in teams of four members. You must stick to the teams submitted in the team submission form. IDs for the submitted teams will be posted on the CMS.
3. Deliverables. You should submit a single .pl file named with your team ID. The sub- mission link will be posted on the CMS prior to the submission.
