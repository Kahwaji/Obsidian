
## Day 1

Transwide: Late but delivered and successful. Peak busy time of the month (all calendar monthly)
Alpega: TMS (Transport Mgmt System) and FX (Freight Exchange - Teleroute). Low amounts / high volumes.
Each Alpega company started with its own billing rules and systems.

Different Billing Systems:
- OpenBravo (WTN only)
- NetSuite
- Billing-Tool: most urgent to replace
- BillingPlatform: new

Migrating Inet and Tender Easy to BillingPlatform is mostly Alpega’s responsibility (with limited on-demand assistance from BP)
### WTN

Started in SugarCRM and “migrated” to Salesforce (and OpenBravo).
OpenBravo has been in use since 2017.
Basic requirement/ overall goal: 
- Simplified/standardized solution as much as possible. 
- Optimize cash flow. Improve financial flow.

WTN volumes:
- 2150- invoices/month
- 8400 lines/month
- Average => ~4 lines/invoice
- 5-10% credits - Mostly full credits. Corrections (partial credits) are rare

Peak processing moments:
- Daily for new customers/contracts 
- 3 times/month for renewals 
- Month-end for usage 
- All usage is invoiced calendar monthly
    
WTN contract information is in SF Opportunity and Subscription. Mostly the Opportunity, which is not yet being referenced by the existing implementation.

(question from Sam) Most painful aspect of the current WTN process - what would you like to see improved?

- New customer onboarding - currently very manual. Would like to see automation (customer, contract and renewals). New sales entered in SF to flow through to BP automatically. 
- Avoid manual work: Annual contracts. Many manual changes throughout the month. 

Change of conditions: Today this is handled in OpenBravo. Involves partial year proration of the old and new subscriptions. This is actually fully calculated in Salesforce automatically(?) and the net proportional  “upgrade” amount is charged for the first portion of the new subscription, falling to the full amount on renewal.

WTN terminology:

- Upsell: add more products (add-ons) to existing contracts/subscriptions
    
- Change of conditions: customer grows and falls into a higher category, where the subscription is more expensive. This could happen mid subscription, causing, effectively, the old subscription to need a partial refund and the new one to be prorated for the first partial period, but this is NOT how it is done (see previous comment – SF calculates net delta amount to be charged for this overlapping period).
    
OpenBravo today upserts Contracts and Invoices into SF (custom SF objects)

Price lists are replicated between SF and OpenBravo. Going forward, price lists will be mastered in Salesforce. WTN thinks they also need to be copied to BillingPlatform for upsells. BP thinks we do not need to have these price lists because upsells (adding more products) would take place in SF and sync’ed to BP with the correct price. No need to replicate prices and pricing logic in BP (SF already has all this).

Price is determined by pricelist version (date) and customer profile.

It will be required to synchronize payment status of all WTN invoices. Not currently being done for any other Alpega businesses. Only payment status being sync’ed are the Proforma Sales Orders, but no invoices. This payment status (not the payments themselves or the amount details in case of partial payments) will be read from Salesforce.

There are Manually created Invoices for a very limited set of products. E.g. Excess views. BP offered to automate calculation, but WTN wants to keep it manual due to the subjective nature of these charges where special concessions are made to customers. Suggestion was to enter these charges in Excel-like files (CSV) and import as usage, in order to automate as much as possible, and not just manually create invoices in BP UI.

Partial credit for annual subscriptions could be for the last X months of the subscription. This could be relevant for credit performance dates. Currently, the credits always reflect the same performance dates as the original charge.

Subscription cancellation auto-credits must be disabled. Any credits will be manually entered as a credit memo.

Account Hierarchies: New account types? Does not seem to be required. All accounts (except for 4) are of type Site in Salesforce.

The Salesforce hierarchy (parent) is not currently being leveraged. Instead, a custom “Invoice To (WTN)” field indicates invoice responsibility delegation, which we might translate to a hierarchy and making lower level accounts non-billable. That way, contracts/account products can be maintained on lower level accounts, while invoices go to the higher Invoice To.

Multi-contract Invoices under 1 account should be consolidated when:
- Checkbox is checked (specifying the desire to consolidate)
- The billing address is the same (this should always be the case if all the charges are going to the same account)
- The postal address (referred to by Alpega as the “ship-to address”)
- Payment conditions (payment terms)
- Payment method (direct pay, etc)
- Payment bank account    


NOTE: For WTN, consolidating multiple contracts under a single invoice is possible. Stefan (and BP) pushed back on this because it does not make business sense to do so, and it is not compatible with the rest of the implementation.

How to identify a WTN account? The WTN PLC (Product Lifecycle) - it is a field indicating whether the account in SF is a WTN suspect, customer, etc.
Subscription cancellation credit => how would the credit look? No such thing is required.
Not many cases when a partial sub cancellation needs to be handled
- Can we disable the subscription credit generation? We should
Usage and fee invoices kept separately
WTN does not currently use SPEOS. Today, they are manually printed, put on envelopes and mailed. They will start using SPEOS.
Print media is currently in the SF Contract, which is NOT currently being used by WTN. But there is a Contract record created in SF and could be used to specify the print media.
Reports: "Forecast" report - built a proto-report in BillingPlatform - For calendar year budgeting. Alpega to review and determine if it is sufficient.

Price increases
- Increase price list rates: SF
- Increase prices for all contracts: BP
- Increase prices for certain customers/products (contracts): BP
- Need to have a filter on location of the customer (Spain vs Portugal) - in BP, when looking for Contracts or Invoices.
- Products shared across Legal Entities (most others are legal entity-specific):
- Connector
- Payment Guarantee

WTN Product Category: Freight Exchange, Private Freight Exchange, GPS, Road Heroes
WTN contracts will never mix products from different categories (important assumption)
4 categories today : makes sense to set the category at the Contract level - for ease of use (search/filters)
When Manually adding a product to an existing contract or manually creating a new invoice, the latest price list rates are required from Salesforce.
Products: Contract items, usage and manual invoices

Product Attributes:
- Main: true/false (customer can only have 1 active sub to one of these)
- Type of Sale: renewable or Puntual (included in the first sale) - Subscription vs One-off or usage (non-recurring)
- Sub Periods: Anual (Yearly), Mensual (Monthly), Trimestral (Quarterly), bi-anual (bi-annual) and Half-yearly.
- Subs are all Advance (PrePaid)
- Templates (?) for usage are kept separate from the non-usage Products
- Items for manually created invoices: currently 2-3 products only. Alpega requires capability to create more.

Customer information - to determine price:
- Type of Company (about 11) and date/version
- When creating "upsells" would happen in BP, BP needs to know the customer profile (level)/seniority and price list prices for that

Active WTN contracts:
- About ~11K
- About 4 items per invoice
    
Usage:
- Total of 13 usage products
- WTN prerates 2 products
- For the rest of the usage products (11), they are applicable to ANY WTN customer. Not explicitly included in contracts and not contract specific
- No need to keep effective-dated rating
- Default pricing in BP required for the (13-2=11) usage products  

Pricing Plan-related info in BP:
- Pricing plans/cards in BP - NO (although Alpega keeps thinking we need them - this needs a final decision)
- Product Category - may need for reports - need to check.
## Day 2

### Morning

WTN Accounts need to be reviewed and enriched.
New daily accounts: about 5-6 new.
If 2 hierarchy versions for the same HQ => multiple VAT

Contract items being handled in different cases:

- Who to invoice: "Invoice To" , Billing Address and VAT
- Shipping (mail) address
- Payment Information
- Payment Terms
- Payment Method
- Payment Bank Account
    
Invoice always goes to the Ledger in NetSuite? 1 account in SF => multiple Customers in NS (?)
- In NS we can't have multiple bank accounts << 

1 Account in SF with 3 contracts with different payment methods, today in OB that splits into multiple "Accounts" - 1 per payment method.
Over 200 of these cases.
Currently in Salesforce, the payment information is not up to date.

Invoice runs (renewals) per recurrence - all recurring charges in a single invoice/contract must have the same recurrence.
Multiple contracts going to a single invoice could creak this, because the service dates might be different, and the recurrences might be different.
Requirement: in a given month, I want to see all the charges that are to be generated that month (any day that month) - based on sub start date/service date.
Requirement: be able to pick/filter on the periodicity and the product category.
90% customers/contracts are annual
9-month period not a requirement. It is in Open Bravo, but it is not used.

### Afternoon

Specify items that need our attention - do not fall into rabbit holes
SF job Filters for accounts need to change/extend to handle the "Invoice To". Sync "Invoice To" account first.
#### Use cases

Evergreen contract: most contracts have no end date. Keep going until the customer decides to cancel, or the contract is terminated for non-payment.
Contract continues: new invoice after each billing period ends. No need to get info from SF. (this is what Alpega defines as “renewal”)
Conditions change: cancel existing SF "subscription" (end date), new opportunity/subscription created in SF. Notes
- Data in SF is not accurate. Because this was not enforced, old subscriptions might not have been closed/ended in these situations (known to have happened).
- SF Data cleanup is required to match OpenBravo. Otherwise avoid sync-ing updates (to old subscriptions) from Salesforce. Alpega thinks such SF cleanup would take at least 6 months.

Price (pricelist) increase:
- For new contracts: always in SF first, then sync to BP.
- For old (legacy non sync) contracts detect changes through SF reports. Use report output to manually reflect changes in BP.
  
Invoice Template: 2 columns that need to be included for WTN: Discount and List Price.
- Discount is in SF Sub Current Fees - Discounts by percentage or fixed amount are possible.
- List price: full price before discounts

## Day 3

For the first year, the payments can be split in payment installments.
Installments only apply to annual.
Within the **Opportunity**, the payment terms are there and must match the list from NetSuite (and BP).
First year and subsequent years payment terms may be different.
This does NOT apply to usage - usage uses the default payment terms, which are at the account level: Net 15.
Some customers are asked to prepay, then they get the invoice that says that it has been paid already
It is also possible the customer is asked to prepay 1 of the installment payments
Opportunity/Subscription with first installment immediately will not be "billable" until it is paid.

IMPORTANT: This means the following:
- 2 Payment Terms per contract:
- First year payment terms
- On renewal payment terms
- If BP synchronizes a billable subscription/opportunity that has a first installment payment of 0 (immediate), that must be assumed to mean that first installment has already been paid and should be presented as such
- Installment Payment terms that have their first period as 0 (immediate) must not be used after the first year.
- NOTE: the information about the “renewal” payment term, does not currently exist in SF. It will be added by Alpega and must not include any “immediate” payment terms
    
Open question about edge case: Subscription change mid-cycle with immediate payment term - what happens?

Refresh about possible contract changes and how Alpega defines them:
- Upsell: add product to existing ongoing contract.
- Conditions change: different if it changes mid-cycle or right on renewal

Auto-sub credit -> NO. Credits are manually created by Alpega in BP.

Free Months: free period offered after the first paid period. The performance start/end dates of the paid period (dates sent to NetSuite for rev rec purposes) must encompass the free months. For example 1 paid year + 1 free month => 13 months from start of the paid period to end of the free month. BP had suggested configuring the periods the other way around: free months first, then paid recurring fees, in order for the recurring fees to remain consecutive and aligned (first and subsequent months). Special logic would be required to calculate the performance dates in these cases
