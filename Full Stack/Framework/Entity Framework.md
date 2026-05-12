detail query
```sql
SELECT top 10

customer_type,

customer_name_eng,

customer_name_kh,

vat_tin,

system,

original_system_reference AS reference,

location,

TRY_CAST(invoice_date AS DATE) invoice_date,

invoice_no,

null dn_cn_date,

null dn_cn_no,

'' item,

TRY_CAST(base_amount_usd AS DECIMAL(18, 6)) AS base_amount,

TRY_CAST(REPLACE(vat, '%', '') AS INT) AS vat,

TRY_CAST(vat_amount AS DECIMAL(18,6)) AS vat_amount,

TRY_CAST(total_usd AS DECIMAL(18,6)) AS total_usd,

TRY_CAST(exchange_rate AS INT) AS exchange_rate,

ROUND(TRY_CAST(total_khr AS DECIMAL(20,0)), 0) AS total_khr,

TRY_CAST(commission_amount AS DECIMAL(18,4)) AS commission_amount,

TRY_CAST(wht AS DECIMAL(18,6)) AS wht,

TRY_CAST(wht_amount AS DECIMAL(18,6)) AS wht_amount,

TRY_CAST(net_cash_usd AS DECIMAL(18,6)) AS net_cash_usd,

TRY_CAST(invoice_date AS DATE) created_at,

''reason,

remark

FROM [ess-dms].dms.central_invoice_6d with (nolock)

WHERE invoice_no like '%HOQ%'

and TRY_CAST(invoice_date AS DATE) >=DATEADD(day,-1, TRY_CAST(GETDATE() as date))

and TRY_CAST(invoice_date AS DATE) < TRY_CAST(GETDATE() as date);

```
summary query

```sql
UNION All
SELECT
    NULL AS customer_type,
    customer_name_en AS customer_name_eng,
    customer_name_kh,
    vat_number AS vat_tin,
    NULL AS system,
    original_sale_invoice_number AS reference,
    location_code AS location,
    TRY_CAST(order_date AS DATE) AS invoice_date,
    invoice_number AS invoice_no,
    NULL AS cn_dn_date,
    NULL AS cn_dn_no,
    0.0 AS base_amount,
    0 AS vat,
    0.0 AS vat_amount,
    0.0 AS total_usd,
    0.0 AS exchange_rate,
    0 AS total_khr,
    0.0 AS commission_amount,
    0.0 AS wht,
    0.0 AS wht_amount,
    0.0 AS net_cash_usd,
    NULL AS inv_url,
    TRY_CAST(order_date AS DATE) AS created_at,
    '' AS reason,
    '' AS remark
FROM [ess-app].sunfe.v_sale_orders_for_invoice WITH (NOLOCK)
WHERE TRY_CAST(order_date AS DATE) >= DATEADD(DAY, -1, CAST(GETDATE() AS DATE))
  AND TRY_CAST(order_date AS DATE) < CAST(GETDATE() AS DATE);
```


