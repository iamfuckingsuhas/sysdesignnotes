

**Round 1: Machine coding**  
1.5 hrs

1. Handle multiple communication request types (Email, SMS, Soundbox, etc.).
2. Different request types have different mandatory input fields (e.g., Email: sender, receiver, subject, message; SMS: mobile number, message)
3. Providers can support one or multiple channels; multiple providers per channel allowed.
4. Each provider can have multiple accounts, segregated for better handling (e.g., critical OTP communication).
5. Randomly select one active provider from eligible providers for each request.

--- 
PhonePe is conducting its annual hackathon and wants to create an interactive platform to host the event. It will be a 2 day event and contestants need to maximize their score over this period by either solving more problems or more difficult problems. Design the backend to support this platform.

**Mandatory Functionalities**

- The system can assume a fixed set of problems. Capability to add problems to the Questions Library is optional
- Contestants should be able to register themselves with their name and their department name.
- A problem should have attributes like description, tag, difficulty level (easy, medium, hard), score.
- Contestants should be able to filter problems based on difficulty level or tags and sort them based on score (design should be extensible to other attributes )
- A contestant should be able to solve a problem as well as get the list of problems solved by him/her.
- A contestant should be able to see the number of users that have solved a given problem and average time taken to solve that problem.
- Scoring strategy for a problem could simply be to award the score assigned for the problem or could be something different like a combination of score and time.
- Display the leaderboard[Important] which can have 2 variants -
- A. Display the top ‘n’ contestants with maximum score and the problems that they have solved.
- B. Display the top ‘n’ departments which have the best scores ( Score of a department will be the sum of scores of all its employees )

**Extension Problem**

- On solving a problem, users should get a recommendation of top 5 problems based on relevance
- The recommendation strategy for problems could simply be similar tags or extensible to include other factors like number of users who have solved a particular problem or a combination of factors ( Design should be extensible).
- Users should be able to get curations like Top 10 most liked problems of a certain tag.

**Capabilities**  
Below are various functions that should be supported with necessary parameters passed.  
These are just suggestions, the signatures can be altered as long as the functionalities are provided.

- Registering a user - addUser()
- Fetch the list of problems - fetchProblems() - Should take as input filtering and sorting criterias and return all matching problems in the right order.
- The result should contain problem attributes like name, tag, difficulty level, score etc.
- Should also display number of users who have solved a problem and the average time taken for that problem
- Solve a problem - solve() - Marks a problem as solved
- [ For the extension problem, this function should return next 5 recommended problems ]
- Fetch solved problems for a user - fetchSolvedProblems() - Fetch the list of solved problems for a user
- Display leaderboard - displayLeaderboard() - Could take as input a flag for either user or department and return the respective leaderboard.

**Guidelines**

- You should store the data in-memory using a language-specific data-structure.
- Implement clear separation between your data layers and service layers.
- Simple and basic function are expected as entry point - no marks for providing fancy * restful API or framework implementation
- Because this is a machine coding round, heavy focus would be given on data modeling, code quality etc, candidate should not focus too much time on algo which compromises with implementation time

**Expectations:**

- Your code should cover all the mandatory functionalities explained above.
- Your code should be executable and clean.
- Your code should be properly refactored, and exceptions should be gracefully handled.
- Appropriate errors should be displayed on the console

**How will you be evaluated?**

- Code Should be working
- Code readability and testability
- Separation Of Concerns
- Abstraction
- Object-Oriented concepts.
- Language proficiency.
- Scalability
- Test Coverage (Bonus Points)
- **[execution time limit] 5 seconds (ts)**
- **[memory limit] 1 GB**

---
Activity tag logging system.  
Functionalities to build:

1. user should be able to add tags to activity in hierarchical order.
2. user should be able to retrive count of how many times tag is used accross all entity

Focused Skills: Design Patterns, Time complexity, Code quality  
After few days code evaluation round was schedule where you have to explain your approach and tradeoffs if any to the interviewer


---
Implement Task Tracking System : Features like Add Task, Get Task, Modify Task, Complete Task, Remove Task, each action should be logged in Acitivity Log, should be able to get logs.

---
1. Flight management - add flights, seats of economy, business, first class
2. Booking management - allow users to search for flights and book them, cancel booking etc
3. Passenger management - allow users to view flights they have booked  
    Focus on readability, concurrency, etc
---
Design and implement a user journey service for monitoring users as they navigate through various use cases. This will be a rule-based journey framework where a user will onboard a journey and move along various stages on the basis of specific conditions.

Requirements:  
Journey:  
journey is a Directed Acyclic Graph (DAG) that represents a predefined path to track a user on.  
A journey consists of 'n' stages, and users must complete specific actions/events to advance to the next stage.  
Multiple users can onboard to the same journey.  
A user can onboard a journey only once.  
There will be complete isolation across different journeys i.e. anything in one journey won’t depend on any other journey.  
Journeys can be of 2 types, time-bound (having a start-date, end-date) and perpetual (no validity and will remain live till it’s active).  
First stage in the journey will be the onboarding stage, middle stages will be the onward stages and the final stage will be the terminal stage.  
We will have to mark the journey as active for it to be picked as part of the evaluation. By default on journey creation, it should be inactive.  
Also when the validity period of a journey is over, mark it inactive for time-bound journeys.

Example:  
There are 2 Journeys:  
J1 : Stage 1 -> Stage 2 -> Stage 3. Onboarding step for Stage 1 is login. For stage 2 is user opening recharge page and Stage 3 is user doing a recharge transaction  
J2: Stage 1 -> Stage 2. Onboarding step is the user opening an upi lite account. For stage 2 is the user doing a top up on their upi lite account.  
Let's say there are 3 users:  
User 1: Logins to the app, hence is on stage 1 of J1. User opens the recharge page, at this time the user moves to stage 2 of J1.  
User 2: Logins to the app, hence is on stage 1 of J1. User opens the recharge page, at this time the user moves to stage 2 of J1 and then does a recharge transaction and is finally moved to stage 3 of J1  
User 3: Logins to the app, hence is on stage 1 of J1. User opens up an upi account and is onboarded to J2.  
So at this point of time, User 1 is at stage 2 of J1, User 2 is at stage 3 of J1 and User 3 is at stage 1 of J1 and stage 1 of J2.

Mandatory Implementations:  
createJourney(Journey journey)  
This will help in creating a journey in the system.  
updateState(String JourneyId, boolean active)  
Mark journey as active/inactive.  
getJourney(String journeyId)  
To get the journey details  
evaluate(String userId, Payload payload)  
Input payload can contain some fixed params and a Map of <key, value>  
Basis the payload, 3 things may happen:  
The payload may match the condition for onboarding the user to a journey. In this case, the user will get onboarded to the journey.

The payload may match the condition required for moving to the next stage for a user already onboarded on a specific stage. In this case, the user will move to the next stage in that journey.  
The payload will not match any condition, in this case it will be simply ignored.  
Users onboarding/transitions in a journey will happen only when the journey is ACTIVE.

5. getCurrentStage(String userId, String journeyId)  
    Find the current stage of a user in the given journey id.
6. isOnboarded(String userId, String journeyId)  
    Return true/false basis on the condition that whether the user is part of the journey or not.

Optional 1:  
Provide a way where we can send SMS to a user as soon as any stage transition happens for that user (onboarding to the journey or moving from one stage to another). It should have the capability to decide for which stage we want to provide this feature.

Optional 2:  
Add support for recurring journeys, where a single user can onboard the journey multiple times.  
Why the journey service?  
This service serves as a platform for creating multiple journeys for different user paths and tracking users along the journey, taking business decisions accordingly in real time without the need of heavy database queries. Additionally, the service can trigger actions when users transition between stages or remain stationary at a particular stage for an extended period. For example, it can send notifications to users who have viewed a product but haven't made a purchase.

System Architecture:  
Imagine all the events of the entire product systems flowing to some message queue, and the journey service acting as one of the consumers of the messages. All the messages flowing in are being evaluated (as per the evaluate(String userId, Payload payload)) and this service keeping track of the user and journey status for real time use case by different product systems using getCurrentStage(String userId, String journeyId)

Points to note:  
Your code should cover all the mandatory functionalities explained above.  
Your code should be executable and clean.  
All necessary validations must be present.  
In case of an exception, proper errorCode must be present.  
Store the data in-memory for journeys and the user-transitions within the journey.

How will you be evaluated?  
Code should be working.  
Code readability and testability  
Separation Of Concerns  
Object-Oriented concepts.  
Language proficiency.  
SOLID principles  
[execution time limit] 0.5 seconds (c)  
[memory limit] 1 GB

The round was of 1.5 hours, you had to write the code and submit on email as zip

---
Problem statement: Design and implement a Multiple Level Cache Management System with N levels: L1,L2,...Ln. Each layer will store Key-Value pairs of data. L1 is the top most layer with the most priority. LN is the last layer with least priority. Each layer has its capacity, read time, write time. Mandatory Requirements: 1. READ KEY Operation: Read will start from L1 level. Keep doing this until the value of KEY is found at any level or the last level has been reached. If the value of KEY is found at any level, then this VALUE should also be written into all previous levels of cache which have higher priority than this level. Total Read time is the sum of Read time of all levels where READ operation was attempted and Write time of all levels where WRITE operation was attempted. You have to print the VALUE of KEY and total time taken to read it. 2. WRITE KEY Operation: Write will start from L1 level. Write the value of KEY at this level and all levels below it. If at any level value of KEY is the same as the given VALUE then don’t write again and return. Total Write time is the sum of WRITE time of all levels where WRITE operation was attempted and Read time of levels where READ operation was attempted. You have to print the total time taken to write this KEY-VALUE information. 3. STAT Operation Stat operations prints three information: a) What is the current usage of each level of cache, i.e. Filled / Total size b) What is the average read time of this Multi-Level Cache System for last 5 READ operation? c) What is the average write time of this Multi-Level Cache System for last 5 WRITE operation? Good to Have/Extensions 1. Implementation of storage change based on Level 2. Implement Cache Manager as a library 3. Extend STAT Operations data points

---
