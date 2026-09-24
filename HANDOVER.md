\# Mystri Track A — Handover



\## Summary



Investigated and repaired the register defects in Track A.



\### Fixes completed



1\. Fixed the Open/Paid invoice filter so each filter returns the correct invoice status.

2\. Fixed invoice identity handling using `(customer\_id, invoice\_number)`.

&#x20;  - Identical invoice records are skipped.

&#x20;  - Conflicting details are rejected.

3\. Fixed row-level validation so one invalid CSV row does not prevent valid rows from being imported.

4\. Fixed payment matching so payments attach only when both `customer\_id` and `invoice\_number` match.

5\. Fixed the import UI so HTTP failures are reported as failures and successful imports display imported/skipped/rejected counts.

6\. Preserved the existing routes, response fields, and fixture data.



\## Verification



Automated tests:



&#x20;   py -m unittest discover -s tests -v



Result:



&#x20;   Ran 5 tests

&#x20;   OK



Manual verification included:



\- Fresh register: 6 invoices, 5 open, INR 3,209.99 outstanding.

\- Open/Paid filtering verified.

\- Duplicate invoice import verified as skipped.

\- Mixed-validity invoice import verified with valid rows processed and invalid rows rejected.

\- Payment import verified:

&#x20; - PAY-201 matched INV-200.

&#x20; - PAY-301 matched INV-300.

&#x20; - PAY-404 remained as an unmatched payment.

\- Browser register refresh verified after imports.



\## Reproduction



Before the fixes, the Open filter returned incorrect records, duplicate invoice imports created duplicate invoices, mixed-validity rows could prevent valid rows from being processed, and payment matching could use amount rather than invoice identity.



After the fixes, the above scenarios behave according to the business rules.



\## Risks / Remaining Notes



No database schema migration was required. Existing routes and fixture structure were preserved.



\## AI / Tool Use



Used ChatGPT (GPT-5.6 Luna) for debugging guidance, code-review suggestions, test planning, and explanations. Suggested changes were reviewed and manually applied. Changes were checked by running the application, reproducing defects, verifying browser/API behavior, and running the automated test suite. Suggestions that did not match observed application behavior were not used.

