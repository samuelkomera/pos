| __Close Report__ | __Day__|
| ------ | ------ |
| __Key__ | __Computation__ |
| Total Payment | Sum of all trans of all orders |
| Cash Total | Sum of all trans of type(cash) of all orders |
| Non Cash Total | Sum of all trans of type(non-cash) of all orders without tips |
| Non Cash Tips | Sum of all tips of all trans type(non-cash) of all orders   |
| Non Cash Totals w/Tips | Non Cash Total + Non Cash Tips  |
| Auto Gratuity | Sum of all the autograt amt of all the checks |
| Gift Card Change| Sum of Change of all trans(Gift Card) of all Orders where order status(Complete) |
| Voucher Change | Sum of Change of all trans(Voucher) of all Orders where order status(Complete) |
| Mobile Change |Sum of Change of all trans(Mobile) of all Orders where order status(Complete) |
| Cash PayIn Totals | Sum of all payIn's where payment status(Complete) and Type(Cash)|
| Cash PayOut Totals | Sum of all payOut's Type(Cash) |
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
| __Grouping Category Summary__ |
| Total  | sum of all trans by catId|
| __Closing Summary__ |
| Credit Cards w/Tips| sums of all trans of type(credit card) |
| Cash Due (From Server/Drawer) |cashIn - (accountTipsPaid +giftcardTipsPaid + voucherTipsPaid +mobileTipsPaid + autogratAmt +giftcardChange + voucherChange +mobileChange) |
| Grand Total | CashDue + Non Cash Totals w/Tips |
