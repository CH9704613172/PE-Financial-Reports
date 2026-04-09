import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# ── Page config ──────────────────────────────────────────────────────────────
st.set_page_config(
    page_title="Private Equity, L.P. — Fund Dashboard",
    page_icon="📈",
    layout="wide",
    initial_sidebar_state="expanded",
)

# ── Custom CSS ────────────────────────────────────────────────────────────────
st.markdown("""
<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@300;400;500;600&display=swap');

/* Global */
html, body, [class*="css"] { font-family: 'DM Sans', sans-serif; }

/* Background */
.stApp { background: #0a0e1a; color: #e8eaf0; }

/* Sidebar */
section[data-testid="stSidebar"] {
    background: linear-gradient(180deg, #0d1220 0%, #111827 100%);
    border-right: 1px solid #1e2d45;
}

/* Metric cards */
.metric-card {
    background: linear-gradient(135deg, #0f1f35 0%, #162033 100%);
    border: 1px solid #1e3a5f;
    border-radius: 16px;
    padding: 24px 20px;
    text-align: center;
    box-shadow: 0 4px 24px rgba(0,0,0,0.4);
    transition: transform 0.2s;
}
.metric-card:hover { transform: translateY(-2px); }
.metric-label {
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: #6b8cae;
    margin-bottom: 8px;
}
.metric-value {
    font-family: 'Playfair Display', serif;
    font-size: 32px;
    font-weight: 700;
    color: #e8f4fd;
    line-height: 1;
}
.metric-sub {
    font-size: 12px;
    color: #4a9eff;
    margin-top: 6px;
    font-weight: 500;
}
.metric-positive { color: #34d399; }
.metric-negative { color: #f87171; }

/* Section headers */
.section-title {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    color: #e8f4fd;
    border-left: 4px solid #4a9eff;
    padding-left: 14px;
    margin: 28px 0 16px 0;
}

/* Hero banner */
.hero-banner {
    background: linear-gradient(135deg, #0f2740 0%, #0a3d62 50%, #1a4a7a 100%);
    border: 1px solid #1e4976;
    border-radius: 20px;
    padding: 36px 40px;
    margin-bottom: 28px;
    position: relative;
    overflow: hidden;
}
.hero-banner::before {
    content: '';
    position: absolute;
    top: -50%;
    right: -10%;
    width: 400px;
    height: 400px;
    background: radial-gradient(circle, rgba(74,158,255,0.08) 0%, transparent 70%);
    border-radius: 50%;
}
.hero-title {
    font-family: 'Playfair Display', serif;
    font-size: 42px;
    font-weight: 900;
    color: #ffffff;
    line-height: 1.1;
    margin-bottom: 8px;
}
.hero-sub {
    font-size: 16px;
    color: #7fb3d3;
    font-weight: 300;
}
.hero-badge {
    display: inline-block;
    background: rgba(74,158,255,0.15);
    border: 1px solid #4a9eff;
    color: #4a9eff;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 1px;
    margin-right: 8px;
    margin-top: 12px;
}

/* Insight box */
.insight-box {
    background: #0f1f35;
    border-left: 4px solid #4a9eff;
    border-radius: 0 12px 12px 0;
    padding: 14px 18px;
    margin: 8px 0;
    font-size: 14px;
    color: #b0c8e0;
    line-height: 1.6;
}
.insight-box strong { color: #e8f4fd; }

/* Warning box */
.warning-box {
    background: #1f1505;
    border-left: 4px solid #f59e0b;
    border-radius: 0 12px 12px 0;
    padding: 14px 18px;
    margin: 8px 0;
    font-size: 14px;
    color: #d4a456;
}

/* Plotly override */
.js-plotly-plot .plotly { background: transparent !important; }

/* Divider */
.fancy-divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, #1e3a5f, transparent);
    margin: 24px 0;
}
</style>
""", unsafe_allow_html=True)

# ══════════════════════════════════════════════════════════════════════════════
# DATA
# ══════════════════════════════════════════════════════════════════════════════

# Portfolio investments
portfolio = pd.DataFrame([
    {"Company": "Pvt Consumer Tech Co. A – Preferred", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Preferred Stock", "Cost": 140_000_000, "FairValue": 169_090_000},
    {"Company": "Pvt Consumer Tech Co. A – Common", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Common Stock", "Cost": 10_000_000, "FairValue": 33_000_000},
    {"Company": "Pvt Consumer Tech Co. A – Notes", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Notes", "Cost": 10_000_000, "FairValue": 12_000_000},
    {"Company": "Pvt Consumer Tech Co. B – Preferred", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Preferred Stock", "Cost": 100_100_000, "FairValue": 120_000_000},
    {"Company": "Pvt Consumer Tech Co. B – Warrants", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Warrants", "Cost": 596_000, "FairValue": 3_000_000},
    {"Company": "Pvt Healthcare Co. A – Preferred", "Sector": "Healthcare", "Geography": "USA", "Type": "Preferred Stock", "Cost": 77_000_000, "FairValue": 95_700_000},
    {"Company": "Pvt Healthcare Co. A – Common", "Sector": "Healthcare", "Geography": "USA", "Type": "Common Stock", "Cost": 25_000_000, "FairValue": 17_500_000},
    {"Company": "Pvt Healthcare Co. B – Contingent", "Sector": "Healthcare", "Geography": "USA", "Type": "Contingent Consideration", "Cost": 50_000, "FairValue": 100_000},
    {"Company": "Blockchain Co. A – Preferred", "Sector": "Blockchain", "Geography": "USA", "Type": "Preferred Stock", "Cost": 3_000_000, "FairValue": 4_300_000},
    {"Company": "Pvt Consumer Tech Co. C – Common", "Sector": "Consumer Tech", "Geography": "China", "Type": "Common Stock", "Cost": 30_000_000, "FairValue": 35_000_000},
    {"Company": "Pvt Consumer Tech Co. C – Notes", "Sector": "Consumer Tech", "Geography": "China", "Type": "Notes", "Cost": 5_000_000, "FairValue": 8_000_000},
    {"Company": "Pvt Consumer Tech Co. C – Warrants", "Sector": "Consumer Tech", "Geography": "China", "Type": "Warrants", "Cost": 900_000, "FairValue": 1_250_000},
    {"Company": "Public Consumer Tech Co. A", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Common Stock", "Cost": 125_000_000, "FairValue": 145_000_000},
    {"Company": "Public Consumer Tech Co. B", "Sector": "Consumer Tech", "Geography": "USA", "Type": "Common Stock", "Cost": 112_750_000, "FairValue": 125_500_000},
    {"Company": "Digital Asset A", "Sector": "Digital Assets", "Geography": "N/A", "Type": "Cryptocurrency", "Cost": 5_000_000, "FairValue": 6_000_000},
    {"Company": "Digital Asset B", "Sector": "Digital Assets", "Geography": "N/A", "Type": "Cryptocurrency", "Cost": 5_000_000, "FairValue": 5_200_000},
])
portfolio["Gain_Loss"] = portfolio["FairValue"] - portfolio["Cost"]
portfolio["Return_Pct"] = ((portfolio["FairValue"] - portfolio["Cost"]) / portfolio["Cost"] * 100).round(1)

# Partners capital waterfall data
partners_data = {
    "General Partner": {"Beginning": 75_884_000, "Contributions": 250_000, "Distributions": -373_000, "Net Income": 8_458_000, "Ending": 84_219_000},
    "Limited Partners": {"Beginning": 682_957_000, "Contributions": 24_750_000, "Distributions": -36_888_000, "Net Income": 32_202_000, "Ending": 703_021_000},
}

# Cash flow summary
cashflow = pd.DataFrame({
    "Category": ["Net Income", "Inv. Purchases", "Digital Asset Purchases", "Inv. Proceeds", "Digital Asset Sales",
                  "Contributions", "Distributions", "Notes Net"],
    "Amount": [40_660_000, -63_589_000, -12_000_000, 91_197_000, 2_000_000,
                24_100_000, -35_131_000, -700_000],
    "Type": ["Operating","Operating","Operating","Operating","Operating","Financing","Financing","Financing"]
})

# P&L data
income_data = {
    "Interest Income": 4_039_000,
    "Dividend Income": 2_495_000,
    "Other Income": 100_000,
    "Management Fee (net)": -7_540_000,
    "Professional Fees": -565_000,
    "Due Diligence": -1_132_000,
    "Broken Deal Costs": -200_000,
    "Interest Expense": -375_000,
}

COLORS = {
    "blue": "#4a9eff",
    "teal": "#34d399",
    "red": "#f87171",
    "gold": "#f59e0b",
    "purple": "#a78bfa",
    "bg": "#0a0e1a",
    "card": "#0f1f35",
    "grid": "#1e3a5f",
}

CHART_LAYOUT = dict(
    paper_bgcolor="rgba(0,0,0,0)",
    plot_bgcolor="rgba(0,0,0,0)",
    font=dict(family="DM Sans", color="#b0c8e0", size=12),
    margin=dict(t=30, b=10, l=10, r=10),
)

# ══════════════════════════════════════════════════════════════════════════════
# SIDEBAR
# ══════════════════════════════════════════════════════════════════════════════
with st.sidebar:
    st.markdown("""
    <div style='text-align:center; padding: 20px 0 10px;'>
        <div style='font-family: Playfair Display, serif; font-size: 20px; font-weight: 900; color: #e8f4fd;'>
            PE, L.P.
        </div>
        <div style='font-size: 11px; color: #6b8cae; letter-spacing: 2px; text-transform: uppercase; margin-top: 4px;'>
            Fund Dashboard
        </div>
    </div>
    """, unsafe_allow_html=True)

    st.markdown("<hr style='border-color:#1e3a5f; margin: 8px 0 20px;'>", unsafe_allow_html=True)

    page = st.radio(
        "Navigate",
        ["🏠 Overview", "📊 Portfolio", "💰 P&L Analysis", "🌊 Cash Flow", "👥 Partners' Capital"],
        label_visibility="collapsed"
    )

    st.markdown("<hr style='border-color:#1e3a5f; margin: 20px 0 16px;'>", unsafe_allow_html=True)
    st.markdown("""
    <div style='font-size:11px; color:#3a5a7a; text-align:center; line-height:1.8;'>
        Fiscal Year End: Dec 31, 20XX<br>
        Based on KPMG Illustrative<br>Financial Statements 2022<br>
        <span style='color:#1e3a5f'>────────────────</span><br>
        All figures in USD
    </div>
    """, unsafe_allow_html=True)

# ══════════════════════════════════════════════════════════════════════════════
# PAGE: OVERVIEW
# ══════════════════════════════════════════════════════════════════════════════
if "Overview" in page:

    st.markdown("""
    <div class="hero-banner">
        <div class="hero-title">Private Equity, L.P.</div>
        <div class="hero-sub">Annual Fund Performance Dashboard — Fiscal Year End</div>
        <div style="margin-top: 16px;">
            <span class="hero-badge">DELAWARE LP</span>
            <span class="hero-badge">US GAAP</span>
            <span class="hero-badge">ASC 946</span>
        </div>
    </div>
    """, unsafe_allow_html=True)

    # KPI Row 1
    c1, c2, c3, c4 = st.columns(4)
    kpis = [
        ("Total Partners' Capital", "$787.2M", "Net assets of the fund", "positive"),
        ("Total Investments (FV)", "$769.4M", "+$130M vs. cost basis", "positive"),
        ("Net Income This Year", "$40.7M", "After all expenses & carried int.", "positive"),
        ("IRR Since Inception (EOY)", "18.6%", "↓ from 23.0% beginning of year", "negative"),
    ]
    for col, (label, value, sub, sign) in zip([c1, c2, c3, c4], kpis):
        sub_class = "metric-positive" if sign == "positive" else "metric-negative"
        col.markdown(f"""
        <div class="metric-card">
            <div class="metric-label">{label}</div>
            <div class="metric-value">{value}</div>
            <div class="metric-sub {sub_class}">{sub}</div>
        </div>""", unsafe_allow_html=True)

    st.markdown("<div class='fancy-divider'></div>", unsafe_allow_html=True)

    # KPI Row 2
    c1, c2, c3, c4 = st.columns(4)
    kpis2 = [
        ("Unrealized Gain on Investments", "$130.0M", "FV $769.4M vs Cost $639.4M", "positive"),
        ("Net Investment Loss", "($3.2M)", "Expenses exceed income", "negative"),
        ("Carried Interest (GP)", "$8.1M", "20% profit share to General Partner", "neutral"),
        ("Cash & Equivalents", "$8.2M", "Up from $4.6M start of year", "positive"),
    ]
    for col, (label, value, sub, sign) in zip([c1, c2, c3, c4], kpis2):
        sub_class = "metric-positive" if sign == "positive" else ("metric-negative" if sign == "negative" else "metric-sub")
        col.markdown(f"""
        <div class="metric-card">
            <div class="metric-label">{label}</div>
            <div class="metric-value" style="font-size:28px;">{value}</div>
            <div class="{sub_class}" style="font-size:12px; margin-top:6px;">{sub}</div>
        </div>""", unsafe_allow_html=True)

    st.markdown("<div class='section-title'>📌 Key Insights for First-Time Readers</div>", unsafe_allow_html=True)

    col1, col2 = st.columns(2)
    with col1:
        st.markdown("""
        <div class="insight-box">💡 <strong>What is this fund?</strong> Private Equity, L.P. is a Delaware limited partnership that invests in private companies (not listed on stock exchanges). It pools money from investors (Limited Partners) and is managed by the General Partner.</div>
        <div class="insight-box">📈 <strong>Are investments profitable?</strong> Yes — the fund invested <strong>$639.4M</strong> and those investments are now worth <strong>$769.4M</strong>, a paper gain of <strong>$130M (20.3%)</strong>.</div>
        <div class="insight-box">🌍 <strong>Where is the money invested?</strong> Primarily in US Consumer Technology (57.8%), US Healthcare (14.4%), and some exposure to China tech and digital assets (crypto).</div>
        """, unsafe_allow_html=True)
    with col2:
        st.markdown("""
        <div class="insight-box">💰 <strong>What is Carried Interest?</strong> The General Partner earns a performance fee called "carried interest" — here $8.1M — which is 20% of profits above a hurdle rate. It's how fund managers are incentivized.</div>
        <div class="warning-box">⚠️ <strong>IRR declined from 23% to 18.6%.</strong> While still a strong return, the declining IRR signals that recent investments are growing slower than earlier ones. Worth monitoring.</div>
        <div class="insight-box">🏦 <strong>Who are the partners?</strong> Limited Partners hold <strong>$703M (89.3%)</strong> of capital and General Partner holds <strong>$84.2M (10.7%)</strong>. LP capital increased $20M this year from new contributions.</div>
        """, unsafe_allow_html=True)

    # Mini charts
    st.markdown("<div class='section-title'>📊 Snapshot Charts</div>", unsafe_allow_html=True)
    col1, col2, col3 = st.columns(3)

    with col1:
        sector_fv = portfolio.groupby("Sector")["FairValue"].sum().reset_index()
        fig = px.pie(sector_fv, values="FairValue", names="Sector",
                     color_discrete_sequence=["#4a9eff","#34d399","#a78bfa","#f59e0b","#f87171"])
        fig.update_layout(**CHART_LAYOUT, title="Portfolio by Sector", height=280,
                          legend=dict(orientation="h", y=-0.2, font=dict(size=10)))
        fig.update_traces(textposition='inside', textinfo='percent', hole=0.4)
        st.plotly_chart(fig, use_container_width=True)

    with col2:
        geo_fv = portfolio[portfolio["Geography"] != "N/A"].groupby("Geography")["FairValue"].sum().reset_index()
        fig2 = px.bar(geo_fv, x="Geography", y="FairValue",
                      color_discrete_sequence=["#4a9eff","#34d399"])
        fig2.update_layout(**CHART_LAYOUT, title="Fair Value by Geography", height=280,
                           xaxis=dict(showgrid=False), yaxis=dict(showgrid=True, gridcolor="#1e3a5f"))
        fig2.update_traces(text=geo_fv["FairValue"].apply(lambda x: f"${x/1e6:.0f}M"), textposition="outside")
        st.plotly_chart(fig2, use_container_width=True)

    with col3:
        cap_data = pd.DataFrame({"Partner": ["General Partner", "Limited Partners"],
                                  "Capital": [84_219_000, 703_021_000]})
        fig3 = px.pie(cap_data, values="Capital", names="Partner",
                      color_discrete_sequence=["#f59e0b","#4a9eff"])
        fig3.update_layout(**CHART_LAYOUT, title="Partners' Capital Split", height=280,
                           legend=dict(orientation="h", y=-0.2, font=dict(size=10)))
        fig3.update_traces(textposition='inside', textinfo='percent+label', hole=0.4)
        st.plotly_chart(fig3, use_container_width=True)


# ══════════════════════════════════════════════════════════════════════════════
# PAGE: PORTFOLIO
# ══════════════════════════════════════════════════════════════════════════════
elif "Portfolio" in page:

    st.markdown("<div class='section-title'>🗂️ Full Portfolio Holdings</div>", unsafe_allow_html=True)

    col1, col2, col3 = st.columns(3)
    sector_filter = col1.multiselect("Filter by Sector", portfolio["Sector"].unique(), default=list(portfolio["Sector"].unique()))
    geo_filter = col2.multiselect("Filter by Geography", portfolio["Geography"].unique(), default=list(portfolio["Geography"].unique()))
    type_filter = col3.multiselect("Filter by Type", portfolio["Type"].unique(), default=list(portfolio["Type"].unique()))

    filtered = portfolio[
        portfolio["Sector"].isin(sector_filter) &
        portfolio["Geography"].isin(geo_filter) &
        portfolio["Type"].isin(type_filter)
    ].copy()

    # Summary cards
    total_cost = filtered["Cost"].sum()
    total_fv = filtered["FairValue"].sum()
    total_gain = total_fv - total_cost
    avg_return = ((total_fv - total_cost) / total_cost * 100) if total_cost > 0 else 0

    c1, c2, c3, c4 = st.columns(4)
    for col, (label, val) in zip([c1, c2, c3, c4], [
        ("Total Cost Basis", f"${total_cost/1e6:.1f}M"),
        ("Total Fair Value", f"${total_fv/1e6:.1f}M"),
        ("Unrealized Gain/Loss", f"${total_gain/1e6:.1f}M"),
        ("Avg. Return", f"{avg_return:.1f}%")
    ]):
        sign = "positive" if "Avg" in label and avg_return >= 0 else ("negative" if "Avg" in label else "positive")
        col.markdown(f"""<div class="metric-card">
            <div class="metric-label">{label}</div>
            <div class="metric-value" style="font-size:26px;">{val}</div>
        </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    # Treemap
    st.markdown("<div class='section-title'>🌳 Portfolio Treemap — Size = Fair Value</div>", unsafe_allow_html=True)
    fig_tree = px.treemap(
        filtered[filtered["FairValue"] > 0],
        path=["Sector", "Company"],
        values="FairValue",
        color="Return_Pct",
        color_continuous_scale=[[0,"#f87171"],[0.5,"#f59e0b"],[1,"#34d399"]],
        color_continuous_midpoint=0,
        custom_data=["Cost", "FairValue", "Return_Pct"]
    )
    fig_tree.update_traces(
        hovertemplate="<b>%{label}</b><br>Fair Value: $%{value:,.0f}<br>Return: %{color:.1f}%<extra></extra>",
        textfont_size=12
    )
    fig_tree.update_layout(**CHART_LAYOUT, height=420, coloraxis_colorbar=dict(
        title="Return %", tickfont=dict(color="#b0c8e0"), titlefont=dict(color="#b0c8e0")))
    st.plotly_chart(fig_tree, use_container_width=True)

    # Bar chart: cost vs FV
    col1, col2 = st.columns(2)
    with col1:
        st.markdown("<div class='section-title'>💵 Cost vs Fair Value by Type</div>", unsafe_allow_html=True)
        type_agg = filtered.groupby("Type")[["Cost","FairValue"]].sum().reset_index().sort_values("FairValue", ascending=True)
        fig_bar = go.Figure()
        fig_bar.add_trace(go.Bar(name="Cost", y=type_agg["Type"], x=type_agg["Cost"],
                                  orientation="h", marker_color="#1e4976"))
        fig_bar.add_trace(go.Bar(name="Fair Value", y=type_agg["Type"], x=type_agg["FairValue"],
                                  orientation="h", marker_color="#4a9eff"))
        fig_bar.update_layout(**CHART_LAYOUT, barmode="overlay", height=340,
                               xaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                               yaxis=dict(showgrid=False), legend=dict(orientation="h", y=1.1))
        st.plotly_chart(fig_bar, use_container_width=True)

    with col2:
        st.markdown("<div class='section-title'>📈 Returns by Investment</div>", unsafe_allow_html=True)
        top = filtered.nlargest(10, "Return_Pct")[["Company","Return_Pct","FairValue"]].sort_values("Return_Pct")
        colors = ["#34d399" if r >= 0 else "#f87171" for r in top["Return_Pct"]]
        fig_ret = go.Figure(go.Bar(
            x=top["Return_Pct"], y=top["Company"].str[:30],
            orientation="h", marker_color=colors,
            text=top["Return_Pct"].apply(lambda x: f"{x:.0f}%"), textposition="outside"
        ))
        fig_ret.update_layout(**CHART_LAYOUT, height=340,
                               xaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                               yaxis=dict(showgrid=False))
        st.plotly_chart(fig_ret, use_container_width=True)

    # Table
    st.markdown("<div class='section-title'>📋 Holdings Table</div>", unsafe_allow_html=True)
    display_df = filtered[["Company","Sector","Geography","Type","Cost","FairValue","Gain_Loss","Return_Pct"]].copy()
    display_df["Cost"] = display_df["Cost"].apply(lambda x: f"${x:,.0f}")
    display_df["FairValue"] = display_df["FairValue"].apply(lambda x: f"${x:,.0f}")
    display_df["Gain_Loss"] = display_df["Gain_Loss"].apply(lambda x: f"+${x:,.0f}" if x >= 0 else f"-${abs(x):,.0f}")
    display_df["Return_Pct"] = display_df["Return_Pct"].apply(lambda x: f"{x:.1f}%")
    display_df.columns = ["Company","Sector","Geography","Type","Cost Basis","Fair Value","Gain / Loss","Return %"]
    st.dataframe(display_df, use_container_width=True, height=380,
                 column_config={"Return %": st.column_config.TextColumn()})


# ══════════════════════════════════════════════════════════════════════════════
# PAGE: P&L
# ══════════════════════════════════════════════════════════════════════════════
elif "P&L" in page:

    st.markdown("<div class='section-title'>📊 Income & Expense Breakdown</div>", unsafe_allow_html=True)

    income_items = {k: v for k, v in income_data.items() if v > 0}
    expense_items = {k: abs(v) for k, v in income_data.items() if v < 0}

    c1, c2, c3 = st.columns(3)
    c1.markdown(f"""<div class="metric-card">
        <div class="metric-label">Total Investment Income</div>
        <div class="metric-value">$6.6M</div>
        <div class="metric-sub metric-positive">Interest + Dividends + Other</div>
    </div>""", unsafe_allow_html=True)
    c2.markdown(f"""<div class="metric-card">
        <div class="metric-label">Total Expenses</div>
        <div class="metric-value">$9.8M</div>
        <div class="metric-sub metric-negative">Mgmt fee is 77% of expenses</div>
    </div>""", unsafe_allow_html=True)
    c3.markdown(f"""<div class="metric-card">
        <div class="metric-label">Net Investment Loss</div>
        <div class="metric-value">($3.2M)</div>
        <div class="metric-sub metric-negative">Expenses > income; normal for PE</div>
    </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    col1, col2 = st.columns(2)

    with col1:
        st.markdown("<div class='section-title'>💸 Income Sources</div>", unsafe_allow_html=True)
        inc_df = pd.DataFrame({"Source": list(income_items.keys()), "Amount": list(income_items.values())})
        fig_inc = px.bar(inc_df, x="Amount", y="Source", orientation="h",
                         color_discrete_sequence=["#34d399"])
        fig_inc.update_traces(text=inc_df["Amount"].apply(lambda x: f"${x/1e6:.2f}M"), textposition="outside")
        fig_inc.update_layout(**CHART_LAYOUT, height=280,
                               xaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                               yaxis=dict(showgrid=False))
        st.plotly_chart(fig_inc, use_container_width=True)

    with col2:
        st.markdown("<div class='section-title'>💳 Expense Breakdown</div>", unsafe_allow_html=True)
        exp_df = pd.DataFrame({"Expense": list(expense_items.keys()), "Amount": list(expense_items.values())})
        fig_exp = px.pie(exp_df, values="Amount", names="Expense",
                         color_discrete_sequence=["#f87171","#f59e0b","#a78bfa","#4a9eff","#34d399"])
        fig_exp.update_layout(**CHART_LAYOUT, height=280,
                               legend=dict(orientation="h", y=-0.3, font=dict(size=10)))
        fig_exp.update_traces(textposition='inside', textinfo='percent', hole=0.3)
        st.plotly_chart(fig_exp, use_container_width=True)

    # Gains breakdown
    st.markdown("<div class='section-title'>🚀 Gains & Net Income Waterfall</div>", unsafe_allow_html=True)
    waterfall_items = {
        "Net Inv. Loss": -3_178_000,
        "Realized Inv. Gains": 25_165_000,
        "Realized Dist. Gain": 200_000,
        "Unrealized Inv. Gains": 17_273_000,
        "FX Realized": 400_000,
        "FX Unrealized": 800_000,
    }
    wf_df = pd.DataFrame({"Item": list(waterfall_items.keys()), "Value": list(waterfall_items.values())})
    measure = ["relative"] * len(wf_df) + ["total"]
    x_vals = list(wf_df["Item"]) + ["Net Income"]
    y_vals = list(wf_df["Value"]) + [40_660_000]

    fig_wf = go.Figure(go.Waterfall(
        name="Net Income Build",
        orientation="v",
        measure=measure,
        x=x_vals,
        y=y_vals,
        texttemplate="%{y:$,.0f}",
        textposition="outside",
        connector=dict(line=dict(color="#1e3a5f", width=1)),
        increasing=dict(marker_color="#34d399"),
        decreasing=dict(marker_color="#f87171"),
        totals=dict(marker_color="#4a9eff"),
    ))
    fig_wf.update_layout(**CHART_LAYOUT, height=380,
                          yaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                          xaxis=dict(showgrid=False))
    st.plotly_chart(fig_wf, use_container_width=True)

    st.markdown("""
    <div class="insight-box">💡 <strong>What does this tell us?</strong> The fund's <strong>net investment loss of $3.2M</strong> means running costs exceed dividend/interest income — this is <em>completely normal</em> for private equity funds. The real returns come from <strong>capital gains</strong>: $25.2M realized + $17.3M unrealized gains drove the fund's $40.7M net income.</div>
    """, unsafe_allow_html=True)


# ══════════════════════════════════════════════════════════════════════════════
# PAGE: CASH FLOW
# ══════════════════════════════════════════════════════════════════════════════
elif "Cash Flow" in page:

    st.markdown("<div class='section-title'>🌊 Cash Flow Statement Analysis</div>", unsafe_allow_html=True)

    c1, c2, c3 = st.columns(3)
    c1.markdown("""<div class="metric-card">
        <div class="metric-label">Net Cash from Operations</div>
        <div class="metric-value">$15.4M</div>
        <div class="metric-sub metric-positive">Healthy operating cash generation</div>
    </div>""", unsafe_allow_html=True)
    c2.markdown("""<div class="metric-card">
        <div class="metric-label">Net Cash from Financing</div>
        <div class="metric-value">($11.7M)</div>
        <div class="metric-sub metric-negative">More distributed than raised</div>
    </div>""", unsafe_allow_html=True)
    c3.markdown("""<div class="metric-card">
        <div class="metric-label">Ending Cash Balance</div>
        <div class="metric-value">$8.2M</div>
        <div class="metric-sub metric-positive">Up $3.7M from prior year</div>
    </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    col1, col2 = st.columns([3,2])

    with col1:
        st.markdown("<div class='section-title'>📉 Cash Flows — Waterfall</div>", unsafe_allow_html=True)
        cf_labels = [
            "Beginning Cash", "Net Income (adj.)", "Inv. Purchases",
            "Digital Asset Purchases", "Inv. Proceeds", "Digital Asset Sales",
            "WC & Other Changes", "LP Contributions", "Distributions Paid",
            "Notes Net", "Ending Cash"
        ]
        cf_values = [
            4_555_000, 40_660_000, -63_589_000,
            -12_000_000, 91_197_000, 2_000_000,
            -25_165_000 + -17_273_000 + -400_000 + -800_000 + 400_000 + -7_000 + 407_000 + 42_000 + 125_000 + -35_000 + 29_000,
            24_100_000, -35_131_000,
            -700_000, 8_215_000
        ]
        measures = ["absolute"] + ["relative"] * 9 + ["total"]

        fig_cf = go.Figure(go.Waterfall(
            orientation="v",
            measure=measures,
            x=cf_labels,
            y=cf_values,
            texttemplate="%{y:$,.0f}",
            textposition="outside",
            textfont=dict(size=10),
            connector=dict(line=dict(color="#1e3a5f")),
            increasing=dict(marker_color="#34d399"),
            decreasing=dict(marker_color="#f87171"),
            totals=dict(marker_color="#4a9eff"),
        ))
        fig_cf.update_layout(**CHART_LAYOUT, height=400,
                              yaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                              xaxis=dict(showgrid=False, tickangle=-30))
        st.plotly_chart(fig_cf, use_container_width=True)

    with col2:
        st.markdown("<div class='section-title'>🧾 Activity Summary</div>", unsafe_allow_html=True)
        activities = [
            ("Investment Purchases", "$63.6M", "negative"),
            ("Investment Proceeds", "$91.2M", "positive"),
            ("Digital Asset Buys", "$12.0M", "negative"),
            ("Digital Asset Sales", "$2.0M", "positive"),
            ("LP Contributions In", "$24.1M", "positive"),
            ("Distributions Out", "$35.1M", "negative"),
            ("Interest Paid (Cash)", "$0.35M", "negative"),
            ("Net Debt Activity", "($0.7M)", "negative"),
        ]
        for label, val, sign in activities:
            color = "#34d399" if sign == "positive" else "#f87171"
            st.markdown(f"""
            <div style='display:flex; justify-content:space-between; padding:10px 16px;
                        background:#0f1f35; border-radius:8px; margin-bottom:6px;
                        border-left: 3px solid {color};'>
                <span style='color:#b0c8e0; font-size:13px;'>{label}</span>
                <span style='color:{color}; font-weight:600; font-size:13px;'>{val}</span>
            </div>""", unsafe_allow_html=True)

        st.markdown("""
        <div class="insight-box" style="margin-top:12px;">
        💡 The fund sold more investments ($91.2M) than it bought ($63.6M), which is typical in a <strong>harvesting phase</strong> of a PE fund life cycle.
        </div>""", unsafe_allow_html=True)


# ══════════════════════════════════════════════════════════════════════════════
# PAGE: PARTNERS' CAPITAL
# ══════════════════════════════════════════════════════════════════════════════
elif "Partners" in page:

    st.markdown("<div class='section-title'>👥 Partners' Capital — Year-over-Year Changes</div>", unsafe_allow_html=True)

    c1, c2, c3, c4 = st.columns(4)
    cards = [
        ("Beginning Capital", "$758.8M", "Both GP + LP combined"),
        ("New Contributions", "$25.0M", "$250K GP / $24.75M LP"),
        ("Distributions Paid", "$37.3M", "Returns to partners"),
        ("Ending Capital", "$787.2M", "+$28.4M net increase"),
    ]
    for col, (label, val, sub) in zip([c1,c2,c3,c4], cards):
        col.markdown(f"""<div class="metric-card">
            <div class="metric-label">{label}</div>
            <div class="metric-value" style="font-size:26px;">{val}</div>
            <div class="metric-sub">{sub}</div>
        </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)
    col1, col2 = st.columns(2)

    with col1:
        st.markdown("<div class='section-title'>📊 Capital Movement Waterfall</div>", unsafe_allow_html=True)
        fig_pw = go.Figure(go.Waterfall(
            orientation="v",
            measure=["absolute","relative","relative","relative","total"],
            x=["Beginning", "Contributions", "Distributions", "Net Income", "Ending"],
            y=[758_841_000, 25_000_000, -37_261_000, 40_660_000, 787_240_000],
            texttemplate="$%{y:,.0f}",
            textposition="outside",
            textfont=dict(size=10),
            connector=dict(line=dict(color="#1e3a5f")),
            increasing=dict(marker_color="#34d399"),
            decreasing=dict(marker_color="#f87171"),
            totals=dict(marker_color="#4a9eff"),
        ))
        fig_pw.update_layout(**CHART_LAYOUT, height=360,
                              yaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                              xaxis=dict(showgrid=False))
        st.plotly_chart(fig_pw, use_container_width=True)

    with col2:
        st.markdown("<div class='section-title'>📑 Net Income Allocation</div>", unsafe_allow_html=True)
        alloc_items = [
            "Net Inv. Loss", "Realized Inv. Gain", "Dist. Gain",
            "Unrealized Gain", "FX Realized", "FX Unrealized", "Carried Interest (GP)"
        ]
        gp_vals = [-31_000, 251_000, 2_000, 173_000, 4_000, 8_000, 8_051_000]
        lp_vals = [-3_147_000, 24_914_000, 198_000, 17_100_000, 396_000, 792_000, -8_051_000]

        fig_alloc = go.Figure()
        fig_alloc.add_trace(go.Bar(name="General Partner", x=alloc_items, y=gp_vals,
                                    marker_color="#f59e0b"))
        fig_alloc.add_trace(go.Bar(name="Limited Partners", x=alloc_items, y=lp_vals,
                                    marker_color="#4a9eff"))
        fig_alloc.update_layout(**CHART_LAYOUT, barmode="group", height=360,
                                 xaxis=dict(showgrid=False, tickangle=-30),
                                 yaxis=dict(showgrid=True, gridcolor="#1e3a5f"),
                                 legend=dict(orientation="h", y=1.1))
        st.plotly_chart(fig_alloc, use_container_width=True)

    # Financial Highlights
    st.markdown("<div class='section-title'>🏆 Financial Highlights (LP Class)</div>", unsafe_allow_html=True)
    highlights = pd.DataFrame({
        "Metric": ["IRR Since Inception (BOY)", "IRR Since Inception (EOY)", "Expenses before Carried Interest",
                   "Carried Interest to GP", "Total Expenses incl. Carried Interest", "Net Investment Loss Ratio"],
        "Value": ["23.0%", "18.6%", "2.6%", "1.1%", "2.5%", "0.5%"],
        "What it means": [
            "Annual return since fund started — beginning of this year",
            "Annual return since fund started — end of this year",
            "Cost to run the fund as % of avg LP capital (before performance fee)",
            "Performance fee paid to GP as % of avg LP capital",
            "All-in cost to LP investors",
            "Interest/dividend income shortfall vs expenses"
        ]
    })
    st.dataframe(highlights, use_container_width=True, height=260,
                 column_config={
                     "Metric": st.column_config.TextColumn(width="medium"),
                     "Value": st.column_config.TextColumn(width="small"),
                     "What it means": st.column_config.TextColumn(width="large"),
                 })

    st.markdown("""
    <div class="insight-box">💡 <strong>Plain-English Summary:</strong> The fund returned <strong>18.6% per year</strong> since inception for Limited Partners — well above typical stock market returns. For every $100 invested, LPs paid $2.50/year in fees and earned roughly $18.60/year in annualized returns. The GP's carried interest of $8.1M is only earned because the fund is performing well (above the 8% hurdle rate).</div>
    """, unsafe_allow_html=True)

# Footer
st.markdown("""
<div style='text-align:center; margin-top:40px; padding:20px; border-top:1px solid #1e3a5f;
     font-size:11px; color:#3a5a7a; letter-spacing:1px;'>
PRIVATE EQUITY, L.P. — ILLUSTRATIVE FINANCIAL STATEMENTS · KPMG 2022 · FOR EDUCATIONAL PURPOSES ONLY
</div>
""", unsafe_allow_html=True)
