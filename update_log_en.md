# Update Log

2019.3.28

Added status field to order for the representation of the sign of order status
When (status & 0x8) is true, it means the current order is cancelled
2019.5.9

Defined that the unit of time involved in the interface is second

2019.5.10

Modified the notice regarding the incorrect parameter of market.deals interface, this interface does not require signature verification

2019.6.3

Modified the interface of order.put\_limit, added is\_fee field as the signal of showing whether the use of deductable tokens is required or not, 1-means use deductable tokens, 0-means do not use
fee\_stock，alt\_fee, deal\_fee\_alt Added 3 fields fee\_stock，alt\_fee, deal\_fee\_alt into order details

fee\_stock:Name of deductable token

alt\_fee:The discount of deductable token

deal\_fee\_alt:The deductable tokens that are already deducted

Modified the status of status field, added the judgment of &0x80 to determine whether deductable tokens are used or not, （status&0x80）TRUE-deductable tokens are used, FALSE-deductable tokens are not used
