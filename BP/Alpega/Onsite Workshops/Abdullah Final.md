
# Day 1

- 5%-10% of customers receive credits each month, mostly due to cancellations.
- Evergreen services remain active until customers request cancellation. No refunds apply.
- Onboarding new customers are migrated from SF to BillingPlatform.
- Master Data:
    - Accounts: SF
    - Contracts (Opportunities & Subscriptions): BP
    - Subscriptions: SF
    - Products: SF
- Contract updates (price changes, upsells, upgrades) must respect the original contract dates.
- A nightly sync process syncs data from OpenBravo to SF.
- BP should not allow price updates—SF is the pricing master.
- Do not auto-credit prorated subscriptions.
- Invoice structure:
    - One invoice → One contract (_Exceptions exist: invoice consolidation "Stefan might be wrong"_)
    - One customer → One legal entity
- Separate product groups/categories per contract.
- Billing Frequency: Monthly, Quarterly, Semi annual, Annual, and 24-month options.
- Billing cycles align with subscription start dates.
- One contract per invoice (there are exceptions like invoice consolidation—"Stefan might be wrong").
- An invoice can run on the 5th of February for an invoice start date of the 15th of February and an end date of the 14th of March, with the close date still being the 5th of February.
- Filtering: BP should allow filtering by customer location (Spain vs. Portugal) when searching for contracts or invoices.
- Product Sync: All products should be created and synchronized in SF, BP, and NetSuite at the same time. BP should not sync products separately

# Day 2

- Scenarios should not be mixed. If mixed, they duplicate the hierarchy. Alpega to check if they have such a scenario.
- Mixed scenarios: BILL TO is the same, but payment & shipping addresses are different, so we must split the HQ invoice based on payment and shipping addresses.
- Abdullah: No need to differentiate all scenarios or between 1 & 3 because invoices are already split based on the above criteria (shipping & payment are different). If there is no BILL TO, it's another invoice split.
- Usage Billing: Usage charges follow the BILL TO invoice.
- Alpega to verify: Do they charge Usage at the BR level, while subscriptions are charged at the HQ level?
- Invoice Processing in NetSuite (NS):
    - Requirement: Different bank details per branch for the same customer.
    - Limitation: NS only allows one bank detail per customer.
    - Possible solutions:
        1. Modify NS to support multiple bank details per customer.
        2. Duplicate the customer entry in NS.

# Contracts

- Automatic contract renewal upon expiration.
- Invoice Close Date: Fixed date when invoices are finalised (e.g., 5th Feb → all Feb invoices are generated and the close date is on 5th Feb).
- Invoice Filtering: Should allow filtering by category (limited to 4 types). No category mixing.
- Invoice Consolidation: Ability to merge invoices for different contract/product cycles (e.g., Annual Product X + Monthly Product Y).
- One-off products do not require contract renewal.
- Invoices should not be sent to SF—instead, SF will use an on demand API to display invoices.
- Contracts should be included in invoice PDFs.
- Upsell/Downsell Process:
    - Managed in SF, with changes synced to BP for processing.
    - Risk: Subscriptions in SF are not properly closed, requiring cleanup before migration (SF vs. OpenBravo).
- Discounts:
    - Based on amount or percentage.
    - Hide discount fields if empty.
    - Display discounts only if non-zero, alongside the list price.
- Usage cannot be discounted.
- Invoice Template:
    - Standardized across all invoices.
    - Includes a new section for WTN.

# Day 3

- Payment Instalments:
    - Setup at the contract level for one or multiple years (in NetSuite).
    - Payment terms (from SF) must be visible on invoices, even if they include multiple subscription types (Annual, Monthly, etc.).
    - Usual practice: Consolidate invoices only for the same period (e.g., Annual).
    - Gus already designed this; we just need to review it.
- Two Payment Terms Exist:
    1. Account level payment terms
    2. Contract level payment terms
- Usage Invoicing:
    - Always follows a 15-day window from the Account-level payment terms.
- Payment Conditions: Defined at the opportunity level.
- Payments will not be stored in BP.
- Invoices should remain open until prepayment is received.
- Prepayment Handling:
    - Prepayment is processed before subscriptions reach BP.
    - Instalments are split equally, including prepayments.
    - No prepayment required on renewals—SF will enforce this before sending data to BP.
    - Data Migration: Alpega must fix prepayment handling before renewals.
- Subscription Linking: Every subscription is linked to an opportunity in SF.
- Prorated Calculations should come from SF, not BP.
- Cancellations:
    - No proration for cancellations.
    - No credit if the customer owes extra.
    - If the customer owes less:
        - If unpaid, apply a credit memo.
        - If paid, roll over the balance to the next contract year.
- Manual Credit Memo Application: Alpega will handle this in BP for unpaid invoices.
- Upsell Definition: For Alpega, upsell means adding new products.
- Change condition/level: A price change (increase or decrease). Happens frequently, every day.
- Change unit: Happens 1-2 times per month. Can occur with change level/condition. Must be done manually since it's not feasible in BP.
   


# Rejoiners

- Rejoiner Policy:
    - Customers who cancel and later rejoin should not be charged for the lost period.
    - Example:
        - Canceled on 5th Feb, rejoined on 20th Feb.
        - They should not be charged for 5th-20th Feb.
        - The renewal date should be 20th Feb, but BP enforces 5th Feb instead.
    - End date consistency: The contract’s end date should remain unchanged in all cases.



# Additional Notes

- Late usage applies to the current month.
- Historical invoices should be disabled. to Check with Gus/Sam how that will not effect Transnet and Teleroute
- Price Increases:
    - Apply to old contracts in BP.
    - Apply to new contracts in SF.

