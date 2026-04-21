
ID: 230042104, 10, 34, 40, 42, 52
---

# Project Title: IUT Hall Dining Management

# Short Description

Students have to pay for the whole year, but they don't actually spend it all. This system allows students to pay only for the food they want to buy or consume.

# Interview Transcript

### What would you like the project name to be?

IUT Hall Dining System

### Purpose?

Cost minimization for students in terms of food, authority budget management simplification, and waste reduction.

### Short Description

Students have to pay for the whole year, but they don't really spend it all. So we make the students only pay for the food they want to buy or consume.

### What do you want?

Auth only for students, simple ID and password, forgot password or something in case they forget.

Login → IUT Email and Password.

Token booking and cancellation feature.

Registration → Requirements

1. Hall ID
2. IUT Mail
3. Then we will allow registration
    1. Then choose a pass
    2. Pay (3000/-) through SSLCommerz

Future feature additions:

1. Show menu to let students decide

### What platform do you prefer?

- Web-based.

### What exactly do you mean by simple password?

- 8 characters

### Special characters, spaces, and max length?

- Just follow Gmail protocol.

### How do you handle forgot password? How do you authenticate?

- Verify using email.
- Provide a link to change the password.

### What if someone uses another person's email address?

- In that case, add verification to confirm if the registering user has access to the provided email.

### Any details you want to add for token purchase?

- Booking → 2 types of tokens:
    1. Weekdays only (Mon to Fri)
    2. All week (including weekend)

Booking starts from Monday, but needs to be booked at least 2 days prior. 1 student, 1 week.

### How do you plan to handle cancellation?

- At least 24 hours prior.
- Always 80% refund.
- Token as QR code for sharing in case the student doesn't want to use it.
- Also a QR code scanner in the hall dining room for the chef to confirm paid students.

### Any range limit for token purchase? Like how far in the future can students buy?

- At a time, only show the option to buy the next 4 weeks' tokens.

### Payment system for token?

- Just use SSLCommerz. It supports most online banking.

### Maybe add the option for paying with IUT ID Card?

- Not now.

### What do you mean by the 2-day and 1-day grace period for booking and cancellation? Does that end at 12 AM or like maybe when IUT starts at 8 AM?

- Just go with 12 AM.

### What about holidays?

- Well, we stay at the halls around that time, so it will not be cancelled.

### Do you want any calendar included in the student dashboard? The calendar will display upcoming holidays and weekdays from the IUT calendar.

- Yes.

### Do you want any admin portal?

- Just provide stats about how many purchases were made this week or this month.
- Ability to delete student access.
- Ability to change registration Fee and Booking Fee.

### Can deleted students re-register?

- Then we create a blocklist for students, and an ability to control that blocklist.

### What about controlling the menu?

- Then we add controls for the menu in the admin. But let's keep that for a future addition.

### Do you want any logs for login and purchase?

- We want purchase records with details.

### Any offer for regular customers?

- No, let's just think about residential students for now.

---

# Identified Requirements

## 1. Authentication & Registration

- Students register using their Hall ID and IUT email.
- Email verification is required during registration to confirm the student owns the email.
- Login is done using IUT email and password.
- Password must follow Gmail protocol - minimum 8 characters, supports special characters, no spaces.
- Forgot password is handled by sending a reset link to the registered IUT email.

## 2. Token Booking

- Two types of weekly tokens:
    - Weekdays only (Monday to Friday)
    - Full week (Monday to Sunday)
- Booking window opens every Monday.
- Students must book at least 2 days prior to the week start, i.e., by 12 AM of the booking deadline.
- Each student can only book one token per week.
- Students can only see and buy tokens for the next 4 upcoming weeks at any given time.

## 3. Token Cancellation & Refund

- Students can cancel a token at least 24 hours before the week starts, by 12 AM.
- Cancellation always gives an 80% refund.
- No full refund under any circumstance.

## 4. Token Sharing via QR Code

- Each token is represented as a QR code.
- Students who don't want to use their token can share the QR code with someone else.
- Hall dining room has a QR code scanner for the chef or staff to verify and confirm paid students at entry.

## 5. Payment

- Payment is handled through SSLCommerz.
- Registration pass costs 3000/-.
- Token purchase is also paid via SSLCommerz.
- IUT ID card payment is not supported for now.

## 6. Student Dashboard

- Students can view and manage their token bookings.
- A calendar is shown on the dashboard displaying upcoming weekdays and holidays based on the IUT academic calendar.
- Students can see their purchase history with full details (Purchase Date and Booking Week and Days).

## 7. Admin Portal

- Admin can view stats: number of token purchases per week and per month.
- Admin can remove a student's access to the system.
- Removed students are added to a blocklist and cannot re-register unless removed from the blocklist.
- Admin can manage the blocklist: add or remove students, change registration fee and Booking fee.
- Menu management will be added in the future, but the admin panel will be ready to support it.

## 8. Future Features (Out of Scope for Now)

- Display daily or weekly menu so students can decide whether to book.
- Menu management controls in the admin portal.
- Payment via IUT ID card.