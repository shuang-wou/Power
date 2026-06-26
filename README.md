# Power BI

import pandas as pd

df = pd.DataFrame({
    "iso": ["FRA", "DEU", "GBR", "ITA", "ESP", "NLD", "LUX", "CHE", "ARE", "SAU"],
    "country": ["France", "Germany", "United Kingdom", "Italy", "Spain", 
                "Netherlands", "Luxembourg", "Switzerland", "United Arab Emirates", "Saudi Arabia"],
    "NV": [35000, 27000, 22000, 16000, 12000, 10000, 9000, 8500, 7000, 6000],
    "LCR_Value": [28000, 21000, 16000, 11000, 8500, 7600, 7200, 6900, 5000, 4300],
    "NSFR_Value": [30000, 23000, 18000, 13000, 9500, 8100, 7800, 7200, 5600, 4800]
})

import plotly.express as px
import plotly.io as pio

pio.renderers.default = "browser"

fig = px.choropleth(
    df,
    locations="iso",
    color="NV",
    hover_name="country",
    hover_data={
        "iso": False,
        "NV": ":,.0f",
        "LCR_Value": ":,.0f",
        "NSFR_Value": ":,.0f"
    },
    color_continuous_scale=[
        [0, "#E8EEF7"],
        [0.3, "#AFC4E8"],
        [0.6, "#4F78B8"],
        [1, "#0B2D5C"]
    ],
    title="EMEA Corporate Funding Footprint"
)

fig.update_geos(
    scope="europe",
    showframe=False,
    showcoastlines=True,
    coastlinecolor="white",
    showland=True,
    landcolor="#F5F7FA",
    showocean=True,
    oceancolor="#FFFFFF",
    showcountries=True,
    countrycolor="white",
    projection_type="natural earth"
)

fig.update_layout(
    title={
        "text": "<b>EMEA Corporate Funding Footprint</b><br><sup>Current funding distribution by client country</sup>",
        "x": 0.02,
        "xanchor": "left"
    },
    font=dict(
        family="Arial",
        size=13,
        color="#1F2937"
    ),
    paper_bgcolor="white",
    plot_bgcolor="white",
    margin=dict(l=20, r=20, t=70, b=20),
    coloraxis_colorbar=dict(
        title="NV",
        tickprefix="€",
        ticksuffix="m"
    )
)

fig.show()




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

