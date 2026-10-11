# AI Payment Tracking Agent

An AI-powered payment tracking automation built with **Zapier Agents,
Google Sheets, and Gmail**. The project helps manage payment requests
and update a payment register when a recipient replies with a clear
payment confirmation.

## Problem Statement

Tracking payment requests manually can be time-consuming. Someone must
identify pending invoices, send the right payment request, read replies,
match each reply to the correct invoice, and update the payment status.

This project demonstrates how a Zapier Agent can assist with that
workflow while keeping Google Sheets as the central payment register.

## What the Agent Does

### 1. Send payment requests

The agent checks eligible rows in the Google Sheet and uses the `Type`
field to choose the recipient:

-   **Maintenance** → `arpannandi2808@gmail.com`
-   **Project** → `arpan559.hitit2020@gmail.com`

The payment request includes the invoice ID, customer name, payment
type, amount, bank name, and account number. It asks the recipient to
reply after payment.

### 2. Read payment confirmation replies

When a payment request reply is available to the agent, it looks for:

-   Payment status
-   Invoice ID
-   Transaction ID, if supplied

The agent should only proceed when the reply clearly confirms that
payment is complete and the invoice can be identified reliably.

### 3. Update the matching spreadsheet row

The agent finds the row whose `Invoice ID` matches the reply and updates
only that row:

-   `Status` → `Paid`
-   `Transaction id` → the supplied transaction ID, when available

If the invoice cannot be matched or the reply is unclear, the agent
should not change the sheet.

## Workflow

``` text
Google Sheets
     |
     | Find a pending payment
     v
Check payment Type
     |
     +---- Maintenance ----> Send request to maintenance recipient
     |
     +---- Project --------> Send request to project recipient
                                  |
                                  v
                         Recipient replies with
                         payment confirmation
                                  |
                                  v
                         Extract Invoice ID,
                         status and Transaction ID
                                  |
                                  v
                         Match Invoice ID in
                         Google Sheets
                                  |
                                  v
                         Update only that row:
                         Status = Paid
                         Transaction id = supplied ID
```

> **Important:** Sending a request and processing a later reply may
> require separate triggers or workflows, depending on how the Zapier
> Agent and its Gmail tools are configured. The prompt alone does not
> create continuous email monitoring. Test the complete flow with sample
> data before relying on it.

## Technology Stack

-   **Zapier Agents** --- workflow instructions and tool orchestration
-   **Google Sheets** --- payment register and status tracking
-   **Gmail** --- payment request emails and confirmation replies

## Google Sheet Structure

Create a spreadsheet named `Agentic Ai(project 1)` with a worksheet
named `Sheet1` and these exact headers:

  Column   Header             Purpose
  -------- ------------------ -------------------------------------------------
  A        `Sr`               Serial number
  B        `Invoice ID`       Unique invoice identifier used to match replies
  C        `Type`             `Maintenance` or `Project`
  D        `Name`             Customer or recipient name
  E        `Amount`           Amount requested
  F        `Account No`       Account number included in the request
  G        `Bank Name`        Bank name
  H        `Status`           `Pending` or `Paid`
  I        `Transaction id`   Transaction reference, if supplied

### Sample data

Use fictional data for development and testing.

  ---------------------------------------------------------------------------------------------------
         Sr   Invoice Type          Name        Amount Account No     Bank    Status    Transaction
                   ID                                                 Name              id
  --------- --------- ------------- -------- --------- -------------- ------- --------- -------------
          1      1001 Maintenance   Madhav       50000 TEST-ACC-001   SBI     Pending   

          2      1002 Project       Ram          28000 TEST-ACC-002   ICICI   Pending   

          3      1003 Maintenance   Pavan        94000 TEST-ACC-003   HDFC    Pending   

          4      1004 Project       Raj          80000 TEST-ACC-004   Kotak   Pending   

          5      1005 Maintenance   Veeru        85000 TEST-ACC-005   DBS     Pending   
  ---------------------------------------------------------------------------------------------------

Do not use real bank account details in a public repository. Replace all
sample values with clearly fictional test data.

## Suggested Confirmation Format

Ask the recipient to reply using a consistent format:

``` text
Payment Status: Paid
Invoice ID: 1001
Transaction ID: TEST-TXN-1001
```

A reply that only says `Paid` is not sufficient to identify the correct
invoice reliably when multiple payment requests are outstanding.

## Setup

1.  Create a Google Sheet using the exact spreadsheet, worksheet, and
    column names documented above.
2.  Add fictional test rows with `Status` set to `Pending`.
3.  In Zapier Agents, create an agent and connect the Google Sheets and
    Gmail tools.
4.  Grant only the permissions needed for the workflow.
5.  Configure the agent instructions to route messages by `Type`, match
    replies by `Invoice ID`, and update only `Status` and
    `Transaction id`.
6.  Ensure the Gmail reply-processing workflow has a suitable trigger or
    is invoked when replies arrive.
7.  Test with one invoice at a time before testing multiple rows.
8.  Confirm that the correct email is sent, the correct invoice is
    matched, and no unrelated rows or columns change.

## Testing Checklist

-   [ ] A `Maintenance` row routes to the maintenance recipient.
-   [ ] A `Project` row routes to the project recipient.
-   [ ] The email includes the correct invoice and payment details.
-   [ ] A clear `Paid` reply with a matching Invoice ID updates the
    correct row.
-   [ ] A supplied Transaction ID is saved in `Transaction id`.
-   [ ] A reply with no reliable Invoice ID does not update the sheet.
-   [ ] An unclear or unconfirmed reply does not mark the payment as
    paid.
-   [ ] Rows already marked `Paid` do not receive duplicate requests.
-   [ ] No unrelated rows or columns are changed.

## Safety and Limitations

-   This is a learning/demo automation, not a payment gateway or bank
    verification system.
-   An email saying that payment is complete is **not proof that funds
    have arrived**. For real financial operations, verify transactions
    through the bank or payment provider before marking them as paid.
-   Do not publish real account numbers, customer information, email
    contents, API keys, or credentials in GitHub.
-   Use test data and test email accounts while developing.
-   Configure duplicate prevention and reliable invoice matching before
    using the workflow beyond a demo.
-   Zapier plan limits and available integrations may vary. Some
    triggers or actions may require a paid plan.

## Future Improvements

-   Add a `Request Sent` or `Email Sent At` column to prevent duplicate
    payment requests reliably.
-   Add a `Confirmation Received At` column for audit history.
-   Add human approval before changing a payment status.
-   Add error handling and notifications for unmatched invoice IDs.
-   Use verified bank/payment-provider data for actual payment
    confirmation.

## Project Purpose

This project is intended to demonstrate practical automation concepts:
tool integration, conditional routing, structured data extraction,
record matching, safe updates, and multi-step workflow design with
Zapier Agents.
