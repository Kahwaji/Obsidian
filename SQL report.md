



SELECT *
FROM (
    SELECT 
        pr.Name AS Product_Name,
        TO_CHAR(a.TransactionDate, 'YYYY-MM') AS Transaction_Date,
        a.CurrencyCode AS Currency,
        nvl(a.CreditAmount,DebitAmount)  AS Amount,
                pr.BPProductType
    FROM GENERAL_LEDGER_TRANSACTION a   
    LEFT JOIN ACTIVITY ac ON ac.Id = a.BillingActivityId 
    LEFT JOIN product pr ON pr.id = ac.productid
    WHERE 1=1
)
PIVOT (
        SUM(Amount) 
    FOR Transaction_Date IN (
        '2020-12' AS Dec_2020,
        '2021-01' AS Jan_2021, '2021-02' AS Feb_2021, '2021-03' AS Mar_2021, '2021-04' AS Apr_2021, 
        '2021-05' AS May_2021, '2021-06' AS Jun_2021, '2021-07' AS Jul_2021, '2021-08' AS Aug_2021, 
        '2021-09' AS Sep_2021, '2021-10' AS Oct_2021, '2021-11' AS Nov_2021, '2021-12' AS Dec_2021,
        '2022-01' AS Jan_2022, '2022-02' AS Feb_2022, '2022-03' AS Mar_2022, '2022-04' AS Apr_2022, 
        '2022-05' AS May_2022, '2022-06' AS Jun_2022, '2022-07' AS Jul_2022, '2022-08' AS Aug_2022, 
        '2022-09' AS Sep_2022, '2022-10' AS Oct_2022, '2022-11' AS Nov_2022, '2022-12' AS Dec_2022,
        '2023-01' AS Jan_2023, '2023-02' AS Feb_2023, '2023-03' AS Mar_2023, '2023-04' AS Apr_2023, 
        '2023-05' AS May_2023, '2023-06' AS Jun_2023, '2023-07' AS Jul_2023, '2023-08' AS Aug_2023, 
        '2023-09' AS Sep_2023, '2023-10' AS Oct_2023, '2023-11' AS Nov_2023, '2023-12' AS Dec_2023,
        '2024-01' AS Jan_2024, '2024-02' AS Feb_2024, '2024-03' AS Mar_2024, '2024-04' AS Apr_2024, 
        '2024-05' AS May_2024, '2024-06' AS Jun_2024, '2024-07' AS Jul_2024, '2024-08' AS Aug_2024, 
        '2024-09' AS Sep_2024, '2024-10' AS Oct_2024, '2024-11' AS Nov_2024, '2024-12' AS Dec_2024,
        '2025-01' AS Jan_2025, '2025-02' AS Feb_2025, '2025-03' AS Mar_2025, '2025-04' AS Apr_2025, 
        '2025-05' AS May_2025, '2025-06' AS Jun_2025, '2025-07' AS Jul_2025, '2025-08' AS Aug_2025, 
        '2025-09' AS Sep_2025, '2025-10' AS Oct_2025, '2025-11' AS Nov_2025, '2025-12' AS Dec_2025,
        '2026-01' AS Jan_2026, '2026-02' AS Feb_2026, '2026-03' AS Mar_2026, '2026-04' AS Apr_2026, 
        '2026-05' AS May_2026, '2026-06' AS Jun_2026, '2026-07' AS Jul_2026, '2026-08' AS Aug_2026, 
        '2026-09' AS Sep_2026, '2026-10' AS Oct_2026, '2026-11' AS Nov_2026, '2026-12' AS Dec_2026,
        '2027-01' AS Jan_2027, '2027-02' AS Feb_2027, '2027-03' AS Mar_2027, '2027-04' AS Apr_2027, 
        '2027-05' AS May_2027, '2027-06' AS Jun_2027, '2027-07' AS Jul_2027, '2027-08' AS Aug_2027, 
        '2027-09' AS Sep_2027, '2027-10' AS Oct_2027, '2027-11' AS Nov_2027, '2027-12' AS Dec_2027,
        '2028-01' AS Jan_2028, '2028-02' AS Feb_2028, '2028-03' AS Mar_2028, '2028-04' AS Apr_2028, 
        '2028-05' AS May_2028, '2028-06' AS Jun_2028, '2028-07' AS Jul_2028, '2028-08' AS Aug_2028, 
        '2028-09' AS Sep_2028, '2028-10' AS Oct_2028, '2028-11' AS Nov_2028, '2028-12' AS Dec_2028
    )
)
ORDER BY 1