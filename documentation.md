# Telecom Billing & Customer Service System

## 1. What is this Project About?

The main goal of this project is to create a simple system for a telecom company to manage its customers and billing. Think of it as a digital assistant for the company's admin staff.

It will help with tasks like:

- Signing up new customers with proper verification
- Keeping track of their mobile plans (like Prepaid or Postpaid)
- Monitoring how much data and call time they use
- Automatically creating and sending monthly bills
- Recording payments and handling service status

## 2. The Main Parts of the System (Modules)

We can break the system down into six main jobs, or "modules":

1. **Customer Management:** Handle all customer information. Add new customers, update details (like a new address), or remove them from the system.
2. **Plan Management:** Manage mobile plans. Create new plans (e.g., a new student offer), change existing ones, or delete old ones.
3. **Usage Tracking:** Record every customer's monthly usage. Track call minutes and gigabytes (GB) of data used.
4. **Billing Module:** Automatically calculate the bill for every customer at the end of each month. Includes plan's monthly fee and charges for extra usage.
5. **Payment Module:** Track payments. See who has paid and who hasn't. Send reminders for overdue bills.
6. **Service Control:** Temporarily pause service for non-payment or reactivate once payment is made.

## 3. How We Store Information (Data Structure)

To make this system work, we need to store information in an organized way. Based on your diagram, here are the main "tables" where we'll keep our data:

- **Customer**
    - Stores basic details for each person: Name, Phone, Email, and a unique national ID (like an Aadhaar number).
    - Each customer has a unique `Cust_ID` to identify them.

- **Plan**
    - Holds details about each mobile plan: Plan Name, Type (Prepaid/Postpaid), Monthly Fee, and limits for calls and data.
    - Each plan has a unique `Plan_ID`.

- **Registered Sim (RegSim)**
    - Links a Customer to a Plan using their `Sim_No`. Shows which customer is using which plan and tracks the SIM's status (Active/Inactive).

- **Usage**
    - Tracks the monthly call minutes and data used by each customer's SIM.
    - Connects to the RegSim table.

- **Bill**
    - Contains information for each monthly bill: Total Amount, Due Date, and Status (Paid/Unpaid).
    - Linked to a specific customer's SIM and their usage for the month.

- **Payment**
    - Records every payment made against a bill.
    - Stores the payment amount, date, and payment method (e.g., UPI, Credit Card).

## 4. Detailed Processes & Case Study

This section explains how key operations are handled in the system.

### Case Study: Customer Registration & SIM Allocation

The process for getting a new SIM is carefully controlled to ensure proper identity verification.

#### Step 1: Identity Verification

Whether the person is a new or existing customer, the first step is to verify their identity using their national ID number (Aadhaar). The system checks the ID's validity.

#### Step 2: Check Existing Connections

The system checks how many SIM cards are already registered to this person's ID with our company. Regulations limit the number of SIMs one person can have (e.g., a maximum of 9).

- If the user is at the limit: The request for a new SIM is denied.
- If the user is below the limit: The process continues.

#### Scenario A: New Customer Registration

1. An admin enters the new customer's details (Name, Address, National ID).
2. After the ID is verified and the SIM limit check passes, a new `Cust_ID` is created in the Customer table.
3. The customer chooses a plan, and a new SIM is issued. This creates a new entry in the RegSim table, linking the `Cust_ID` to the `Plan_ID` and the new `Sim_No`.

#### Scenario B: Existing Customer Gets a New SIM

1. An admin looks up the existing customer using their phone number or `Cust_ID`.
2. The system performs the same SIM limit check using their stored national ID.
3. If the check passes, the customer chooses a plan for the new SIM. A new entry is added to the RegSim table for the new SIM, linked to their existing `Cust_ID`.

### Billing and Payment Cycle

The billing process is automated to ensure accuracy and timeliness.

1. **Bill Generation:** On a fixed day each month (e.g., the 1st), the Billing Module runs automatically. For each active postpaid customer, it calculates the total amount due, including:
        - The fixed monthly plan fee
        - Charges for any data or call minutes used beyond the plan's limit
        - Applicable taxes and surcharges
2. **Bill Notification:** Once the bill is generated in the Bill table, the customer is notified via SMS and email with the total amount and due date.
3. **Payment Reminders:** If the bill is not paid a few days before the due date, an automated reminder is sent. A second reminder is sent if the due date passes and the bill remains unpaid.
4. **Marking Payments:** When a customer pays (via UPI, card, etc.), an admin records it in the Payment table. This automatically updates the corresponding bill's status to "Paid" in the Bill table.

### Service Deactivation and Reactivation

Service control is critical for managing non-paying customers and other issues.

**A service can be temporarily deactivated for the following reasons:**

- **Non-Payment:** If a bill is not paid within a certain period after the due date (e.g., 15 days), the service is automatically deactivated.
- **Customer Request:** A customer can request to temporarily suspend their service (e.g., if they are traveling abroad).
- **Security Reasons:** If fraudulent activity is detected from the number, the service can be suspended pending investigation.

**Reactivating a Service:**

- For Non-Payment: The service is reactivated once the customer clears all outstanding dues. A small reconnection fee might also be applied to their next bill.
- For Customer Request: The service is reactivated on the date specified by the customer or when they request it.

**Permanent Disconnection:**

If a customer's account remains unpaid and the service deactivated for an extended period (e.g., 90 days), the number may be permanently disconnected and can be reassigned to a new customer in the future.
