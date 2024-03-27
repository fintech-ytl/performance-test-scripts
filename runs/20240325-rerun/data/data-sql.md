filename: interbankTransferData.csv
columns: customer_id,fund_option_id,account_id (account number)
query:

1. To get main csv
(from deposit database)
```sql
SELECT ao.customer_id, fov.fund_option_id, fov.account_id FROM fund_option_view fov 
	JOIN demand_deposit_account dda ON fov.account_id = dda."number" 
	JOIN account_owner ao ON ao.account_id = dda.id 
WHERE dda.current_balance_amount > 10.00
ORDER BY dda.created_at DESC;
```
2. To get external target
```sql
SELECT fund_option_id  FROM fund_option_view fov WHERE TYPE = 'INDIVIDUAL_BANK_SAVINGS_EXTERNAL';
```

filename: duitnowCreditWebhookData.csv
columns: number (accountNumber)
query:
(from deposit database)
1. To get main csv
```sql
SELECT "number" FROM demand_deposit_account dda WHERE status = 'ACTIVE' LIMIT 500;
```

filename: transfereeFavouritedAndUnfavouritedData.csv
columns: customer_id, transferee_id
query:
(from payment)
1. 
```sql
SELECT DISTINCT ON (customer_id) customer_id, id AS "transferee_id" FROM transferee t;
```