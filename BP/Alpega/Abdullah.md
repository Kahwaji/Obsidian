
Day 1:
- 5%- 10% of customers get credited every months. Most of the cases are due to cancellation
- Services are like evergreen, get cancelled when customers ask. No refund for that.
- onboarding new customers from SF to Billingplatform
- Master data for account is SF, for Contract it's in BP
- Updating contracts for new price/upsell/ upgrades respects the old contract dates.
- Daily nightly process to sync data from OpenBravo to SF.
- We should not allow updating the prices in BP as the Price master is SF.
- Not to auto credit propertied subscription
- One invoice -> One contract (there are exception like invoice consolidation) -> one customer -> one Legal Entity
- Separate products groups/ category per contract
- One contract per invoice (there are exception like invoice consolidation)
- Invoice can run on 5th Feb for invoice start date as of 15th Feb and end date 14th Mar and the close date is still the 5th Feb
- All products should be created in SF,BP and NetSuite at the same time and in sync already, BP shouldn't sync them.


Day 2:
- Diff between Scenario 1 & 3? they will have the same BILL TO, so how to diff them?
- Scenarios should not be mixed, if mixed, they duplicate the hierarchy. Alpega to check if they have such scenario.
- Mixed scenarios: Bill To same, Payment & shipping add are diff so we have to split the HQ invoice based on payment and shipping address.
- Abdullah: no need to diff all scenarios or between 1 & 3 because we are splitting invoices based on the above criteria anyway (shipping & payment are different). If it has no BILL TO, t's another invoice split
- Usage will follow BILL TO invoice
- Alpega to check if they have a case where they charge Usage at BR level where the subscriptions charge the HQ level.

- The requirement is to send invoices with different bank details (on branch level) for the same customer to NS.  
- The limitation lies in NS, where each customer can only have one set of bank details per customer.  
- There are two possible solutions:  
  1. Modify NS to allow different bank details for a single customer in some way (ex: The one customer should be duplicated in NS).


Contracts:
- Contracts to be renewed automatically when it ends.
- Invoice close date is fixed date where they suppose to run the invoices. Ex: 5th ofFeb will have all Feb invoices generated and closed as 5th Feb as close date.
- Ability to filter invoices based on invoice category (only 4 types) , no category mix will be there.
- Ability to consolidate invoices for diff contracts/products cycle (Ex: annual product X with monthly product Y)
- No renewal for one off products
- No need to send invoices to SF, the SF will have an API to run to display invoices on demand when needed.
- Add contracts to invoices PDF
- Upsell/down-sell happen in SF. All changes will be synced to BP to take necessary actions. note/risk: subscriptions SF are not closed in SF. Needs cleaning (SF va Open bravo) before migration.
- Discounts: Amount or %
- To hide discount fields if null
- Same invoice template for all -> new section for WTN
- Show discounts if not zero with list price otherwise hide both if discount is zero.
- Usage can't be discounted


Day 3:
- Set up payments instalments on contract level for one or many years (in NetSuite). Payment terms to show on the invoice (source SF) even if the invoice contains many diff subscriptions (Annual, monthly etc). Usually they consolidate only the same periods in one invoice (Annual). Gus already designed this , we just need to review.
- They have 2 payment terms: Account payment terms and Contract payment terms. For usage: it's always 15 days from Account payment terms level.
- Payment condition is on opportunity level.
- Payment will not be stored in BP.
- Invoices should be be closable until prepayment is received.
- Prepayment is already paid before subscription sent to BP
- payments instalments are already split equally including the prepayment
- Prepayments (Payment term 0) should not be there on renewals. SF will inforce this rule before sending anything to BP. For Data migration, Alpega needs to fix it before the renewal
- Every subscription is linked to Oppo. in SF.
- The prorated calculation should come from SF not to be done by BP
- No proration on cancellation and no credit if the client has to pay extra. if it has to pay less, the apply a credit memo if not paid, if paid, move it to next year's contract.
- No credit for cancellation 
- Credit memo will applied manually from Alpega in BP for not paid invoices.
- Upsell for them is only adding products for Alpega
- Change condition/level : is a price change up or down. It's frequent, everyday. 
- Change unit happens 1-2 a month. It could happen with change level/condition. It has to happen manually now as it's not feasible in BP.
- rejoiners: they want to renew rejoiners and don't want to charge the customer for the lost period. Ex: cancelled on 5th Feb, rejoined on 20th FEB, the don't wanna charge the customer between 5th and 20th and the renewal date should be 20th FEB, which is not possible in BP. in BP it must be 5th Feb. The end date of contract should be same in all cases.
- 

- Late usage goes to the current month.
- Historical invoice should be disabled.
- price increase: for old contracts in BP and new Contracts in SF

