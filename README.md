# Power BI

UI Design

1. Advanced Slicer with changing color

   More visuals - Chiclet Slicer
2. Dynamic text

   Selected Country =
"Current Country: " &
SELECTEDVALUE(Sales[Country], "All Countries")

3. Different level budget

Current  Budget = 
VAR CurrentL3=selectedvalue('Astre Analytical View'[ALM_BUSINESS_3])
VAR CurrentL4=selectedvalue('Astre Analytical View'[ALM_BUSINESS_4])
VAR CurrentL5=selectedvalue('Astre Analytical View'[ALM_BUSINESS_5])
VAR Budget2=
   calculate(sum('Budget'[T Current Budget]),
             'Budget'[ALM5]=CurrentL5,
              Budget[ALM3]=CurrentL3,
   
             'Budget'[ALM4]=CurrentL4) 
VAR Budget1=
   calculate(sum('Budget'[T Current Budget]),
              Budget[ALM3]=CurrentL3,
             'Budget'[ALM4]=CurrentL4)
VAR Budget0=
   calculate(sum('Budget'[T Current Budget]),
            
             'Budget'[ALM3]=CurrentL3)            
VAR Budget2T=
   calculate(sum('Budget'[T Current Budget]),
             'Budget'[ALM3]=CurrentL3,
             'Budget'[ALM4]=CurrentL4,
             NOT ISBLANK(Budget[ALM5]))
VAR Budget1T=
   calculate(sum('Budget'[T Current Budget]),
            
             'Budget'[ALM3]=CurrentL3,
             NOT ISBLANK(Budget[ALM4]))

return
switch(true(),
isinscope('Astre Analytical View'[ALM_BUSINESS_5]),Budget2,
isinscope('Astre Analytical View'[ALM_BUSINESS_4]),coalesce(Budget1,Budget2T),
isinscope('Astre Analytical View'[ALM_BUSINESS_3]),coalesce(Budget0,Budget1T),
sumx(values('Astre Analytical View'[ALM_BUSINESS_3]), Budget0))


RETURN
IF(
    NOT ISINSCOPE('Astre Analytical View'[ALM_BUSINESS_3]),
    SUMX(
        VALUES('Astre Analytical View'[ALM_BUSINESS_3]),
        CALCULATE([Current Budget])
    ),
    SWITCH(
        TRUE(),
        ISINSCOPE('Astre Analytical View'[ALM_BUSINESS_5]), Budget2,
        ISINSCOPE('Astre Analytical View'[ALM_BUSINESS_4]), COALESCE(Budget1, Budget2T),
        ISINSCOPE('Astre Analytical View'[ALM_BUSINESS_3]), COALESCE(Budget0, Budget1T)
    )
)

4. Ignore reportfilter
CALCULATE(
    SUM(Sales[Amount]),
    ALL(Customer[Country])
)



import plotly.express as px
import pandas as pd

df = pd.DataFrame({
    "country": ["France", "Germany", "China", "United States", "Singapore"],
    "NV": [35000, 27000, 20000, 15000, 8000]
})

fig = px.choropleth(
    df,
    locations="country",
    locationmode="country names",
    color="NV",
    hover_name="country",
    color_continuous_scale="Blues",
    title="Corporate Funding by Country"
)

fig.show()

