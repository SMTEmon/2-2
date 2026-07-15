---
Date: 7/5/26
---
### Example NFR: 
The system should be user- friendly. -> Too Vague
- (should -> shall) 
- (user-friendly -> within 3 mins, they can apply for leave)

so, The System shall be like within 3 mins, they can apply for leave. (still too vague. Whos they??)
- The system shall be like within 3 mins, students can apply for leave. (anyone might need 3x time for using the service for the first time)
	- The system shall allow a first time user to submit a leave application within the first 3 min after logging in, without external assistance

### Example 2
The system shall allow students to register, login & update their profile.
- Need to test multiple atomic action just to verify 1 NFR. So split them up.

### Example 3
The system shall allow student to reset passwords.
- How?
- Where?
- reset whos password?
- allow all students to reset pass?
-> The system shall allow registered students to reset their own passwords.

### Example 4
The system shall have an attractive UI.
- How do you quantify attractiveness. 

### Example 5
The system shall use MongoDB to store student records.
- This is technical Req, not a NFR.
- Unless the client specifies that this req is needed/ must, this will not be on NFR


## Characteristics
1. Clear
2. Unambiguous
3. Complete
4. Consistent
5. Verifiable
6. Feasible
7. Necessary
8. Modular/ Atomic
9. Traceable (we do not need to specify all of this all the time)
	1. Unique ID (FR-001)
	2. Req
	3. Source
	4. Reason
	5. Priority
10. Modifiable (for continuous development)


### Example 6
The system shall allow users to search for book quickly.
- Search for book? How? Title?
- Quickly -> Can't Quantify
- Asking for 2 req in 1 NFR

## Rules for each NFR
- Shall/ Should
- No Vague Words
- One req per sentence
- Don't combine FR & NFR
	- Sometimes both might be combined, but should be rare case
- Specify Actor (User)
- Specify Condition
	- The system should show the payment receipt (NO)
		- After the payment is done, the system shall show the payment receipt
- Specify Data
	- The System shall store student info
		- What info?
- Avoid design decision unless they are real constraints
	- The MongoDB Example "The system shall use MongoDB to store student records.", no need to say it in NFR unless they are constraints
- Add Rationals / Reasons


## EARS format for NFR
Easy approach to Requirement Syntax

### Patterns
- While {something}, the system shall {something}
- The system shall {something}
- Ubiquitous Req (something that applies to multiple features/ funciton??) (defines how the system as a whole should behave, not just for a single feature)
- Event driven Req: when {something}. the system shall {something}
- State- driven Req: while {something}, the system shall {something}
- where{something}, the system shall {something}
- Unwanted Behavior Req: if {something}, then the system shall {something}
	- Used when we are facing some unwanted behavior
	- Example: if the student enters wrong pass 5 times, then lockout ...