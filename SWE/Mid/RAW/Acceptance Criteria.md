- Define "Done".


## Example 1
The system shall allow registered users to login with username & password.



## Atlasian??? 
Predefined requirements & conditions that a product/ task must meet to be marked complete & accepted by the user.

1. Remove ambiguity
2. Helps developers to understand boundaries
3. Helps testers to create test cases
4. Helps customers Accept/ Reject Failures


Format? (for small projects)
FR- 01: ...
	AC- 01.1:...
	AC- 01.2:...
	AC- 01.3:...



 
| FR  | Summary User Req | AC         |
| --- | ---------------- | ---------- |
| 01  | ...              | 01.1, 01.2 |
| 02  | ...              | 02.1, 02.2 |


## Format for writting Acceptance Criteria
### 1. CheckList Format
(missed when to use)

1. To-Do
2. To-Do
### 2. Gherkin Format
If theres multiple diff options/ scenarios/ branches for the same task, use this
- Given (initial condition)
	- When (user action)
		- Then (system output/ expected Result)


### Example
The system shall allow registered students to register for courses during the registration period. 
- Here what happens when the student is not loggged in?




- Have 5-10 AC for each FR 
	- Too little AC leaves room for ambiguity
	- Too much AC makes the client/ reader confused.
		- If theres too much, break down the FR's 


### Types of Acceptance Criteria
- Positive AC
	- When things go as intended (user is actually logged in before accessing)
- Negative AC
	- When things don't go as intended (user don't have access/ not logged in while trying to access features)
	- For creating edge test cases
