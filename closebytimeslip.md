| __Close Report__ | __Timeslip(based on primary->head timeslipId)__|
| ------ | ------ |
| __Key__ | __Computation value__ |
| __Payment Summary__ |
| Payment | sum of quantity of trans  and type of trans  |
| Sales | sum of total trans by type with status(complete) |
| Tip | sum of total tips of all transactions by type with status(complete) |
| Total | Sales + Tip |
| __Payment Detail__ |
| Payment | Type of trans and order Id |
| Sales | Trans by type with status(complete) |
| Tip | Tip for each trans of an order  |
| Total | Sales + Tip |
| __Server Banking Payment Reconcilation__ |
| Sales Revenue | GCDue + AccountDue + sum of trans type (CC) + sum of trans(voucher) + sum of trans type(cash) + Discount |
| Non Cash Total | Sum of all trans of type(non-cash) of all orders without tips |
| Cash Total | Sum of all trans of type(cash) of all orders |
| Cash PayIn Totals | Sum of all payIn's where payment status(Complete) and Type(Cash)|
| Cash PayOut Totals | Sum of all payOut's Type(Cash) |
| Gross Cash| Sum of all trans of type(Cash) + Cash PayIn Totals - Cash Payout Totals|
| Auto Gratuity | Sum of all the autograt amt of all the checks |
| Gift Card Change| Sum of Change of all trans(Gift Card) of all Orders where order status(Complete) |
| Voucher Change | Sum of Change of all trans(Voucher) of all Orders where order status(Complete) |
| Mobile Change |Sum of Change of all trans(Mobile) of all Orders where order status(Complete) |
| Cash Due from (Server/Drawer) | cashIn - (accountTipsPaid +giftcardTipsPaid + voucherTipsPaid +mobileTipsPaid + autogratAmt +giftcardChange + voucherChange +mobileChange)|
| Deposit Amount | moneyOut - moneyIn |
| Open Cash | moneyIn |
| Close Cash | moneyOut |
| Over/Short | DepositAmount-cashDue |
| __Coupon / Discount Summary__ |
| Total Discount | Sum of all discounts applied by type(discount) for all orders|
| __Pay In/Out Summary__ |
| Total Pay Ins | Sum of all payIns's where payment status(Complete) |
| Total Pay Outs | Sum of all payOut's|
| Net PayIn/Outs | Total Pay Ins - Total Pay Outs|
| __Comp / Void Summary__ |
| Total Comps | sum of all pcompc of items of all orders |
| Total Voids | sum of all pnetc of items of all orders  |
| __Sales Reconciliation__ |
| Net Sales | sum of all pnetc of items of all orders |
| Other Sales | sum of all gcnet of items of all orders 
| Taxes | sum of all check taxTotals |
| Gross Sales | Net Sales + Other Sales + Taxes |
| Guest Count | sum of count of guests of all orders |
| Guest Average | GrossSales/GuestCount |
| Bar Mix| Percentage based on the sales of menuCats actual related to rule|
| __Grouping Category Summary__ |
| Total  | sum of all trans by catId|
| __Closing Summary__ |
| Credit Cards w/Tips| sums of all trans of type(credit card) |
| Voucher| sums of all trans of type(voucher) |
| Cash Due (From Server/Drawer) |cashIn - (accountTipsPaid +giftcardTipsPaid + voucherTipsPaid +mobileTipsPaid + autogratAmt +giftcardChange + voucherChange +mobileChange) |
| Grand Total | CashDue + Non Cash Totals w/Tips |
