# Analyzing-unicorns-companies

Hand with calculator
Did you know that the average return from investing in stocks is 10% per year (not accounting for inflation)? But who wants to be average?!

You have been asked to support an investment firm by analyzing trends in high-growth companies. They are interested in understanding which industries are producing the highest valuations and the rate at which new high-value companies are emerging. Providing them with this information gives them a competitive insight as to industry trends and how they should structure their portfolio looking forward.

You have been given access to their unicorns database, which contains the following tables:

dates
Column	Description
company_id	A unique ID for the company.
date_joined	The date that the company became a unicorn.
year_founded	The year that the company was founded.
funding
Column	Description
company_id	A unique ID for the company.
valuation	Company value in US dollars.
funding	The amount of funding raised in US dollars.
select_investors	A list of key investors in the company.
industries
Column	Description
company_id	A unique ID for the company.
industry	The industry that the company operates in.
companies
Column	Description
company_id	A unique ID for the company.
company	The name of the company.
city	The city where the company is headquartered.
country	The country where the company is headquartered.
continent	The continent where the company is headquartered.
The output
Your query should return a table in the following format:

industry	year	num_unicorns	average_valuation_billions
industry1	2021	---	---
industry2	2020	---	---
industry3	2019	---	---
industry1	2021	---	---
industry2	2020	---	---
industry3	2019	---	---
industry1	2021	---	---
industry2	2020	---	---
industry3	2019	---	---
Where industry1, industry2, and industry3 are the three top-performing industries.

available as
df

_billions
1
WITH top_industries AS
2
(
3
    SELECT i.industry,
4
        COUNT(*) -- Removed i,* and replaced with *
5
    FROM industries AS i
6
    INNER JOIN dates AS d
7
        ON i.company_id = d.company_id
8
    WHERE EXTRACT(year FROM d.date_joined) IN ('2019','2020', '2021')
9
    GROUP BY industry
10
    ORDER BY count DESC
11
LIMIT 3
12
),
13
​
14
yearly_rankings AS
15
(
16
    SELECT COUNT(*) AS num_unicorns, -- Removed i,* and replaced with *
17
        i.industry,
18
        EXTRACT(year FROM d.date_joined) AS year,
19
        AVG(f.valuation) AS average_valuation
20
    FROM industries AS i
21
    INNER JOIN dates AS d
22
        ON i.company_id = d.company_id
23
    INNER JOIN funding AS f
24
        ON d.company_id = f.company_id
25
    GROUP BY industry, year
26
)
27
​
28
SELECT industry,
29
    year,
30
    num_unicorns,
31
    ROUND(AVG(average_valuation / 1000000000), 2) AS average_valuation_billions
32
FROM yearly_rankings
33
WHERE year IN ('2019', '2020', '2021')
34
    AND industry in (SELECT industry
35
                    FROM top_industries)
36
GROUP BY industry, num_unicorns, year
37
ORDER BY year DESC, num_unicorns DESC
industry
year
num_unicorns
average_valuation_billions
0
Fintech
2021
138
2.75
1
Internet software & services
2021
119
2.15
2
E-commerce & direct-to-consumer
2021
47
2.47
3
Internet software & services
2020
20
4.35
4
E-commerce & direct-to-consumer
2020
16
4
5
Fintech
2020
15
4.33
6
Fintech
2019
20
6.8
7
Internet software & services
2019
13
4.23
8
E-commerce & direct-to-consumer
2019
12
2.58
