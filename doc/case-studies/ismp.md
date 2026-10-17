## Case Study: SMPorres — Tuition Collections System

**Context.** A technical-career institute in Argentina charged an enrollment fee and monthly installments, with scholarships, early-payment discounts and late fees, and collected everything through a bank. Staff reconciled the payments by hand against a legacy Access database, so nobody could say reliably who had paid, who owed what, or how much interest a late payer should be charged.

**Solution.** A set of applications over one SQL Server database: a Windows Forms back office for the staff, an ASP.NET MVC portal where students check their account and print payment slips, a REST API for enrolling students from an external system, and two scheduled jobs for debt notices and database backups to Google Drive.

**My role.** I co-developed the system with a friend and authored about 70% of the commits. 

### Key Technical Decisions

**1. Late fees computed in tranches, not with a single rate.** The first version applied one monthly rate to the whole delay. That breaks as soon as the rate changes, so it was replaced with a rates table with start and end dates: a payment made months late is charged each tranche at the rate in force at the time, with the daily rate taken as the monthly rate divided by 30. Scholarships add a second variable, because a scholarship either stops applying after the due date or applies always, depending on the student's policy, and the fee is computed on the amount that policy leaves. The desktop app and the web portal each implement the calculation, so the rates table is synchronized to the portal database to keep both results identical.

**2. A bank-compatible barcode and automatic reconciliation.** Each payment slip carries a barcode built from the collector code, the payment id, the due date in Julian format (year plus day of year) and the amount, followed by a check digit, according to the specification agreed with the bank. That makes the bank's settlement file self-describing: the import screen reads it, matches every line to an installment, flags the lines whose amount does not match, lets the operator review the result, and saves only the valid payments in a single transaction. Loading the same file twice triggers a warning.

**3. Separate databases for the back office and the public portal.** The student portal has its own database with only the data students need, pushed from the desktop application and the API through a SOAP web service, so the internal collections database never sits behind the public site. The trade-off is that the copy is updated after the transaction commits: if the push fails, the data is already saved in the back office, and the API reports exactly that instead of pretending the operation failed.

### Outcome

Delivered to the client during 2019 and 2020, covering the full cycle from enrollment and payment plans to bank reconciliation, debt notices and a self-service portal. The repository keeps the SQL migrations in dated scripts, the requirements and payment rules agreed with the client, and the bank homologation material.

### What I would do differently today

The late-fee calculation lives in two places and should be a shared library, the SOAP service would become a REST endpoint, and the solution has no automated tests, which I would add first around the payment calculations.