Minutes:  
  
company overview:  
transport management company  
  
Overview/EcoSystem:  
subscription based, usage based and onetime fee pricing models  
5 different companies under single umbrella  
3 different billing systems Transwide is live=BillingPlatform (Bruno), Inet = Netsuite billing, OpenBravo = current billing system  
Open Bravo former ERP currently use/ current billing system  
SalesForce = using connector  



Volume:  
2k invoices per month, 8400 charges   
1.5 fte on the billing team  
5-10% of the invoices are credited  
majority cancelation  
2043 credit memos were created last year  
refunds are not common  
credit automation already built = file upload  
rate = default rate on the product or list price  
price = contracted rate  
  
Points of Pain:  
1. onboarding new customers contract automation  
2. avoid manual work  
  
Data Masters:  
  
accounts = salesforce  
subscriptions = salesforce  
contracts = salesforce in the form of opportunity and subscriptions, custom objects to store the contract after it is created in BP  
products = salesforce  
invoices/credits = billingplatform  
payments/GL = netsuite  
usage = wtn  
  
OpenBravo:  
Prices/Discounts are maintained  
Contracts are maintained in open bravo and synced to SFDC  
  
  
Payment Status = information only used for reporting   
Manual Invoice = additional charges/fixed fees or overage, these will come in the form of usage and uploaded into BP   
Auto expiration of contracts is happening in openbravo  
  
Products:  
- Need to be able to group products by Categories to be used for price increases and other functionality  
Categories: Freight Exchange, Private Freight Exchange, GPS, Road Heroes  
Pricing Types:  
Onetime fees,   
Subscription,   
Puntual = One Time  
  
- SFDC will be product master  
- Frequency: Monthly, Quarterly, Semi-Annual, Annual, 24 months  
- Billing cycles are aligned with the start date of the subscription  
- all products have to be created in SFDC and synced to BP and Netsuite from SFDC  
  
Discounts:  
- Discount amount is included on the subscription or line item in Salesforce. The record will include list price, discount amount and unit price  
- Discounts are not displayed as separate line items on the invoice  
- Discount may be limited to a certain period i.e. first year only. There is a field on the subscription (discount period) that defines the number of invoice periods discount will be applicable to. After expiration BP should bill for the subscription using list price instead of unit price  
  
Customer Accounts:  
11000 active customers  
- New fields: Invoice To  
- New Account Types: Site, Company, Null, National (99% of accounts are Sites)   
- Account Type should never be blank or null. the data will need to be cleaned up before loading to BP  
- In SFDC On the subscription there is a field called Product that will define the wtn subscription/contract  
- each account and contract will need to be enriched after creation. decision?  
- need ability to identify WTN specific contracts/subscription  
- single customer account could have contracts from multiple business channels  
  
Contracts:  
- 11000 active contracts, 40000 contract rates  
- no cross company/legal entity contracts  
- single contract per product category  
- separate invoices per contract (decision?)  
- ability to filter invoices by product category  
- single contract can only have one product category  
- need ability to generate charges and invoice for specific product billing frequencies (should not be a requirement)  
- no support for 9-months subscription  
- one time charges only occur on the initial invoice on the new customer  
- usage fees have to be on the separate invoices  
- the subscriptions with different billing frequencies cannot be on the same contract  
- majority of the contracts are evergreen  
- the contract cancellation or early terminations should NOT produce prorated credit. t  
- no refunds are issued for unused portion of the subscription in the case of the early termination  
- when necessary Billing team will issue a Manual Invoice Line Item credit for the unused portion of the subscription  
  
- NEW:  
Created in SFDC and synched to BP via connector  
  
  
- RENEWAL  
Auto Renewal - evergreen contract auto renews with the same terms/rates defined on the creation   
  
- MODIFICATION/AMENDMENT:  
  
Amendment Types:  

Change Level (2-3 daily) = change conditions, price changes (up/down) = in sfdc the original opportunity is canceled and new one is created. The new opportunity is created with a net amount of the difference between original and a new one prorated up to the renewal  
  
Change Entity (2 month) = changes to the customer name, legal attributes i.e VAT,   
Customer is canceled in SFDC, Subscription is closed, The new account is created, new opportunity is created, new contract is created,   
Acquisitions are OOS these are not valid use cases.
Upsell = Adding new product(s) to the existing contract. Completed in SFDC and pushed to BP via connector.
RollOut/Customer retention =
To know the appropriate type we need to evaluate: Business Type, Business SubType, Close Reason, Close sub reason fields in SFDC to know what type of the opportunity it is.  
  
Hierarchies:  
-Need to keep billing hierarchy for Billable accounts  
  
Invoices:  
INVOICE SPLIT/Consolidation:  
- Ability to consolidate charges from multiple contracts from different subaccounts into a single invoice for the HQ  
- Usage - 13 unique products, 2 are pre-rated, 11 are not and BP will need to keep the default price  
- Usage products are not part of the contract and are billed on the separate invoices from subscriptions and   
- scenario where the branch accounts use the same billing address as HQ for the VAT calculation  
- Speos the print vendor will be used to send mail invoices  
- The charges to be split by billing frequency of the contract to a separate invoice  
- the payment terms are defined on the contract and account. The usage and other non-contracted charges use payment terms from account and the charges produced from the contract use payment terms from contract  
  
Free Month:  
- 1-2 months free tagged to the end of the subscription period  
- Netsuite needs to see the single subscription with an extended period i.e. 13 months   
  
Manual invoice:  
- need to be able to bill certain adhoc charges i.e. bank fees on demand  
  
Invoice template  
- need ability to display list price, discount and unit price just for WTN   
- need ability to hide discount and list price if discount does not apply  
- discounts do not apply to usage charges  
- in the payment details need to display the payment terms split into multiple installements for annual subscription only  
  
Salesforce:  
- new account with active subscription need to be synced  
- Opportunity and Subscriptions need to be synced  
- amendments Upsell/Downgrade need to be synced for all new subscriptions created post go live  
- Pricing will be maintained in SFDC and synced to BP  
- Invoice details are displayed in SFDC for the users (no pdf) via API  
- No price changes in BP - SFDC is a price master  
  
Netsuite:  
- sync invoices and credit to NS  
- sync invoice payment status to BP  
- sync accounts/contracts to NS  
  
Usage:  
- usage comes from WTN in the form of the csv file and is loaded onto ftp, BP will need to grab the newly uploaded file and process it  
- usage is processed in the beginning of each month for prior month  
- usage could include backdated transactions  
- all usage records need to be invoiced on the most recent open invoice/billing period and no historical invoices should be created  
- at the minimum usage will need to include the following fields: CustomerAccountId, Usage Date, Invoice Date, Quantity, Product, Rate Override (when applicable)  
  
Data Migration:  
- migrate accounts (SFDC)  
- migrate products (SFDC)  
- no prior invoices or credits will be migrated and will remain in OpenBravo  
- migrate contracts (OpenBravo)  
- migrate default prices (11 usage products only)  
- major issues with SFDC data. The subscriptions do not align between OpenBravo and SFDC. This is a high risk item and does prevent BP from automating contract user stories related to modifications, cancellations and amendments  
- how to migrate multiple accounts into one? currently the accounts get duplicated in both OpenBravo and NS when payment details (bank account, aba, and etc) are different on individual contracts  
- open expired subscriptions in SFDC that should no longer be billed  
- existing contracts will come from openbravo and will not be synced from SFDC because of the data inconsistencies. All contracts/subscriptions prior to go-live will be marked as DO NOT SYNC and will be excluded from SFDC jobs in   
  
Price Maintenance:  
- default rates/list prices will remain in SFDC  
- billingplatform will not keep default rates on the products (only for 11 usage)  
-   
  
Price Increases:   
- done once a year in the beginning of the year and effect the future charges only  
- could be % or fixed amount  
- need ability to export all existing price records for active contracts, update prices and reload them back into BP as a new price record  
  
Reports:  
Revenue Forecasting? one year at the time show all of the recurring charges that will be active in this year(could report requirement)

