# Power BI

UI Design

1. Advanced Slicer with changing color

   More visuals - Chiclet Slicer
2. Dynamic text

   Selected Country =
"Current Country: " &
SELECTEDVALUE(Sales[Country], "All Countries")

3. Different level budget

Budget :=
VAR CurrentL0 = SELECTEDVALUE(Dim_Desk[L0])
VAR CurrentL1 = SELECTEDVALUE(Dim_Desk[L1])
VAR CurrentL2 = SELECTEDVALUE(Dim_Desk[L2])

VAR Budget_L2 =
    CALCULATE(
        SUM(Fact_Budget[Budget]),
        FILTER(
            ALL(Fact_Budget),
            Fact_Budget[L0] = CurrentL0 &&
            Fact_Budget[L1] = CurrentL1 &&
            Fact_Budget[L2] = CurrentL2
        )
    )

VAR Budget_L1 =
    CALCULATE(
        SUM(Fact_Budget[Budget]),
        FILTER(
            ALL(Fact_Budget),
            Fact_Budget[L0] = CurrentL0 &&
            Fact_Budget[L1] = CurrentL1 &&
            ISBLANK(Fact_Budget[L2])
        )
    )

VAR Budget_L0 =
    CALCULATE(
        SUM(Fact_Budget[Budget]),
        FILTER(
            ALL(Fact_Budget),
            Fact_Budget[L0] = CurrentL0 &&
            ISBLANK(Fact_Budget[L1]) &&
            ISBLANK(Fact_Budget[L2])
        )
    )

VAR Budget_L2_Total =
    CALCULATE(
        SUM(Fact_Budget[Budget]),
        FILTER(
            ALL(Fact_Budget),
            Fact_Budget[L0] = CurrentL0 &&
            Fact_Budget[L1] = CurrentL1 &&
            NOT ISBLANK(Fact_Budget[L2])
        )
    )

VAR Budget_L1_Total =
    CALCULATE(
        SUM(Fact_Budget[Budget]),
        FILTER(
            ALL(Fact_Budget),
            Fact_Budget[L0] = CurrentL0 &&
            NOT ISBLANK(Fact_Budget[L1])
        )
    )

RETURN
SWITCH(
    TRUE(),
    ISINSCOPE(Dim_Desk[L2]), Budget_L2,
    ISINSCOPE(Dim_Desk[L1]), COALESCE(Budget_L1, Budget_L2_Total),
    ISINSCOPE(Dim_Desk[L0]), COALESCE(Budget_L0, Budget_L1_Total)
)
4. Ignore reportfilter
CALCULATE(
    SUM(Sales[Amount]),
    ALL(Customer[Country])
)

