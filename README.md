SELECT  
    a.AQA_WINNERID_C AS [Account ID],
    MAX(c.FUND_REP__C) AS [Rep Name],
    COUNT(CASE WHEN t.STATUS = 'Completed' AND t.OWNERID IN (
        '0055x00000Buu7iAAB',
        '005Nt000002xQDSIA2',
        '005Nt000004EdVRIA0',
        '005Nt000005in8bIAA',
        '005Nt000005k98fIAA',
        '005Nt000005xVcoIAE',
        '005Nt000005xWqbIAE',
        '005Nt000006B8q1IAC',
        '005Nt000007RQwrIAG',
        '005Nt000007kh18IAA'
    ) THEN 1 ELSE NULL END) AS [Completed Visits], 
    p.ProgramRank AS [Rank], 
    a.LIFETIME_GIVING__C AS [Lifetime Giving],
    MAX(i.Name) AS [Name], 
    MAX(i.Phone) AS [Phone], 
    MAX(i.BILLINGSTREET) AS [Address], 
    MAX(i.BILLINGCITY) AS [City], 
    MAX(i.BILLINGSTATE) AS [State], 
    MAX(i.BILLINGPOSTALCODE) AS [Zip] 
FROM 
    [ro].[vw_prodcopy_account_attributes] a
JOIN 
    [ro].[vw_prodcopy_contact_attributes] c
ON 
    a.AQA_WINNERIDC = c.AQANUMBER_C
LEFT JOIN 
    [ro].[vw_prodcopy_Task] t
ON 
    t.accountid = a.FULL_ACCOUNT_ID__C
LEFT JOIN 
    [ro].[vw_ods_Paciolan_Priority_Points] p 
ON 
    p.Accountid = a.AQA_WINNERID_C
LEFT JOIN 
    [ro].[vw_prodcopy_account] i
ON 
   i.ID = a.FULL_ACCOUNT_ID__C
WHERE 
    a.AQA_WINNERSOURCEID_C = 'Paciolan' 
    AND a.DONOR_RANK__C BETWEEN 1 AND 10000
    AND p.Prioritypointsprogramid = 'BDCN' 
GROUP BY 
    a.AQA_WINNERID_C, 
    a.LIFETIME_GIVING__C, 
    p.ProgramRank;
