# MP SR Zone

**MP SR Zone — Next Candle Engine** is a TradingView Pine Script v6 overlay indicator for short-term chart analysis. It combines pivot-based support and resistance, a directional next-candle scoring model, price-action signals, higher-timeframe context, multi-timeframe engulfing monitoring, and optional BOS/CHOCH structure labels.

The script is designed with 1-minute and 2-minute workflows in mind, but it does not restrict which chart timeframe can be used. It is an analytical tool, not an automated trading system or a guarantee of future price movement.

## Contents

- [Features](#features)
- [Install on TradingView](#install-on-tradingview)
- [How the signals work](#how-the-signals-work)
- [Feature reference](#feature-reference)
- [Settings reference](#settings-reference)
- [Alerts](#alerts)
- [Behavior and limitations](#behavior-and-limitations)
- [Risk notice](#risk-notice)

## Features

- Pivot-based support and resistance zones with optional ATR, candle, touch-qualified, supply, demand, and high-volume variants.
- Configurable swing-based Smart SnR levels.
- A directional score and quality filter for the next chart candle, shown in an on-chart dashboard.
- EMA trend context, RSI, stochastic momentum, price structure, volatility, support/resistance location, session context, exhaustion risk, and conflict checks.
- Optional legacy CALL/PUT setups near support or resistance and separate breakout conditions.
- Current-chart bullish and bearish engulfing markers, bar colors, and alerts.
- A multi-timeframe engulfing watcher for 5-, 15-, 30-, and 45-minute intervals, with optional Fibonacci levels for detected patterns.
- Optional BOS/CHOCH labels based on confirmed swing breaks.
- A configurable Order Block Finder with historical bullish/bearish zones, latest-level channels, an optional levels panel, and detection alerts.
- Optional trendlines connecting the latest confirmed swing highs and lows, with movable latest-swing markers and separate visibility controls.
- Bullish and bearish three-candle fair value gaps (FVGs) and optional candle-to-candle imbalance zones, with per-direction counts, colors, and extension settings.
- Configurable local swing-high and swing-low levels, a potential next swing target, and previous 4H, daily, weekly, and monthly high/low references.

## Install on TradingView

1. Open TradingView and select a chart.
2. Open **Pine Editor** and paste the contents of the `MP SR Zone` script into the editor.
3. Select **Save**, then **Add to chart**.
4. Open the indicator's settings to configure display, filters, timeframes, and alerts.

No external libraries or services are required. The script runs inside TradingView and uses the chart's symbol and data feed.

## How the signals work

### Next-candle direction

The engine builds separate bullish and bearish scores from trend, momentum, short-term structure, candle shape, volatility, higher-timeframe context, price location, and session context. It applies exhaustion and conflicting-evidence checks, calculates the difference between the two directional scores, and estimates a market-quality value.

The dashboard reports **UP**, **DOWN**, or **NO TRADE**. Plot markers, when enabled, use **BUY** for UP and **SELL** for DOWN. Strong directional signals are identified internally using the strong-edge and strong-quality thresholds; the dashboard's direction remains UP or DOWN rather than displaying a separate strong label.

The dashboard is recalculated from the current chart bar, so the prediction can change while that bar is forming. Next-candle alerts are restricted to confirmed chart bars. A prediction is not a promise that the next bar will move in that direction.

### Choosing a workflow

- **Scalping:** The indicator is designed with 1-minute and 2-minute charts in mind. Use the support/resistance zones, confirmed swing structure, local EMA and momentum context, and optional higher-timeframe filter as a combined view. Signals can change before the active candle closes; use confirmed alerts when close confirmation matters. The **Scalp mode** setting is currently reserved and does not automatically change calculations.
- **Swing trading:** The script can be used on other chart timeframes. Confirmed pivots, trendlines, BOS/CHOCH, Smart SnR, order blocks, and the multi-timeframe context can help map structure and areas of interest. Pivot-based features appear only after their right-side confirmation bars. Review the configured HTF and MTF engulfing intervals, since those are fixed choices in the settings and may not match every swing horizon.
- **Binary-trading context:** UP/DOWN and BUY/SELL labels are directional chart estimates, not predictions of a binary contract's settlement at a particular expiry. The indicator does not model expiry timing, payout, broker pricing, or contract rules. A NO TRADE state and independent risk controls remain important; no signal guarantees an outcome.

These workflows describe ways to read the indicator, not trading instructions or evidence of profitability. Test settings on the intended market and timeframe before relying on them.

### BOS and CHOCH

The **MP BOS/CHOCH** section tracks confirmed swing highs and lows using its own swing-length setting. A confirmed close through the latest unbroken swing is labeled:

- **BOS** when it agrees with the prior structure direction, or when no prior break direction has been established.
- **CHOCH** when it breaks against the prior structure direction.

Each tracked swing level is marked at most once. Labels and lines are drawn only after the break bar closes. BOS/CHOCH is a chart-structure display; it does not currently contribute to the next-candle score or generate its own alert.

### Trendlines and latest swings

The **Show trendlines** control connects the latest two confirmed swing highs and the latest two confirmed swing lows, extending each line to the right. The **Show latest swing markers** control separately displays a single movable **SH** marker and **SL** marker at the most recently confirmed swing points. Both use the configured pivot length, so a swing is identified only after the selected number of bars to its right have closed.

The separate **Swing Levels & Target** group draws horizontal levels from a confirmed three-candle local high or low. Each side has its own visibility, retention count, color, and extension controls. These local levels use a one-bar confirmation pattern and are distinct from the pivot-length trendlines and their SH/SL markers.

When **Show next target** is enabled, the indicator looks for the newest retained swing high above price while the current candle is rising, or the newest retained swing low below price while it is falling. The dashed line is a potential reference level, not a forecast or an order instruction.

### FVGs, imbalances, and previous timeframe levels

Bullish FVGs are identified when the current candle's low is above the high from two bars earlier; bearish FVGs use the inverse condition. Optional bullish and bearish imbalance zones mark a gap between the previous close and current open. FVGs and imbalances are confirmed at bar close. Their boxes include a midpoint line and label, and each direction has independent visibility, retention, color, and extension settings. **None** ends at detection, **Limited** projects by the selected number of bars, and **Default** follows the current chart edge.

Previous 4H, day, week, and month levels use the last completed candle of each interval, so the reference prices remain stable as the new interval develops. Each high and low can be toggled separately. A level is shown only when its timeframe is equal to or higher than the chart timeframe.

### Order Block Finder

A bullish order block is the last down candle before the configured number of up candles; a bearish block is the last up candle before the configured number of down candles. The optional minimum-move filter measures the percent change from the candidate block's close to the last confirming candle. Zones are drawn on the original block candle once the confirming bar closes. Source-candle volume for the latest bullish and bearish blocks is shown on the chart by default and is also available in the Data Window; **Show source candle volume** controls the chart labels. The latest bullish and bearish channels, an optional levels panel, and separate detection alerts are also available. Detection alerts identify chart areas only and are not buy or sell signals.

### Engulfing patterns and timeframe watcher

The current-chart pattern checks candle direction and a sweep beyond the previous candle's high or low, followed by a close beyond the previous candle's open. The optional **Filter engulfing** setting also requires the previous candle to be the opposite color. The pattern marker and bar color can respond to a still-forming candle and may disappear before its close.

The watcher can monitor 5m, 15m, 30m, and 45m intervals that are equal to or higher than the chart timeframe. The watcher is independent of the **Show TF levels** setting. Status can reflect a developing higher-timeframe pattern and may change until that timeframe closes.

When enabled, Fibonacci levels are drawn across the engulfing candle's high-low range at 0%, 25%, 50%, 75%, and 100%. Levels use the selected direction, colors, styles, label option, and extension behavior. **Keep previous level lines** controls whether earlier levels are retained when a new pattern is drawn.

## Feature reference

### Zones and support/resistance

Confirmed chart pivots are used to update core support and resistance. Their width is based on ATR and the signal-zone multiplier. Optional zone types include ATR bands, candle-based zones, zones qualified by repeated touches, and supply/demand zones qualified by an impulse-size threshold. High-volume boxes use the volume recorded on the pivot candle and compare it with recent volume at that pivot. The pivot's formatted volume is displayed by default. Their separate half-width, bar length, fill transparency, and label toggle keep these overlays smaller and less prominent than the candles; the box width uses ATR from its pivot candle. On Forex symbols, TradingView volume is generally broker-specific tick volume, not a consolidated measure of all traded contracts.

The zone engine also calculates proximity to support and resistance for legacy setups and the next-candle model. Pivot detection requires right-side bars to confirm a swing, so a new pivot is not known at the pivot candle itself.

### Smart SnR

Smart SnR derives swing levels from its configured swing period and can display several support and resistance levels. Line style, width, extension, and support/resistance colors are configurable. The current implementation draws the **Swing HiLo** variant; the **Volume** choice is present in the settings but does not currently select a separate volume-based algorithm.

### Legacy setups and breakouts

Legacy CALL and PUT conditions combine proximity to a support/resistance zone, trend and momentum filters, a candle-rejection check, and a three-candle pattern after a zone touch. A three-bar cooldown limits repeated setup signals. Their chart labels require **Show signal markers** and **Show legacy setup markers**.

Bullish and bearish breakout conditions use a close beyond core resistance or support by the configured ATR-based signal-zone distance, plus candle direction. Breakout labels require **Show signal markers** and **Show breakout markers**.

### Dashboard

The main dashboard can display the current direction, edge and quality, local trend, higher-timeframe bias, market session, up/down scores, RSI, and session label. **Show reason codes** adds a chart label with the current explanation for a directional signal or a no-trade result.

The engulfing watcher is a separate table. Its position and text size can be configured independently from the main dashboard.

## Settings reference

Settings are organized in TradingView under these groups:

| Group | Purpose |
| --- | --- |
| **01 • General** | Master display switch, marker visibility, bar coloring, and zone labels. |
| **02 • Scalping Zone Engine** | Core zone visibility and dimensions, pivot/ATR parameters, high-volume box visibility, half-width, length, transparency and labels, Smart SnR, confirmed swing markers, and swing trendlines. |
| **03 • Zone Strength** | Touch count, touch lookback, and ATR-based touch tolerance. |
| **04 • Supply / Demand** | Impulse threshold for supply/demand zones. |
| **05 • Trend / Bias** | Local EMA filter, EMA lengths, and EMA plots. |
| **06 • Higher Timeframe Filter** | HTF timeframe, EMA lengths, and confirmed-EMA selection. |
| **07 • Momentum** | RSI and stochastic thresholds plus the breakout enable switch. |
| **08 • Structure** | Structure lookback used by the scoring model. The **MP BOS/CHOCH** controls are also shown in this area. |
| **09 • Price Action** | Rejection, engulfing confirmation, wick/body ratio, candle-body quality, and close-bias thresholds. |
| **10 • Volatility** | ATR-per-price volatility thresholds and ATR averaging length. |
| **11 • Support / Resistance** | ATR buffer used for entry/location checks. |
| **12 • Session** | Optional session contribution to signal scoring. |
| **13 • Exhaustion** | Candle-body and EMA-distance thresholds for exhaustion checks. |
| **14 • Signal Filter** | Minimum edge and quality, strong-signal thresholds, and conflict threshold. |
| **15 • Dashboard** | Main dashboard visibility and reason-code labels. |
| **16 • Alerts** | Enable/disable gate for next-candle alerts. |
| **17 • Backtest / Statistics** | Reserved statistics control; statistics are not currently implemented. |
| **18 • MTF Engulfing Levels** | Enabled intervals, pattern filtering, watcher display, Fibonacci levels, markers, coloring, and engulfing alerts. |
| **19 • Order Block Finder** | Confirmation periods, minimum move, candle range, color scheme, marker/source-volume/channel visibility, zone fill, optional panel and its position, documentation tooltip, and alerts. |
| **20 • FVG & Imbalances** | Bullish/bearish FVG and imbalance visibility, retention counts, colors, extension modes and lengths, and zone transparency. |
| **21 • Swing Levels & Target** | Swing-high/low level visibility, retention, colors and extension, plus the potential next-target display and colors. |
| **22 • Previous HTF High / Low** | Independent previous 4H/day/week/month high and low visibility and timeframe colors. |
| **MP BOS/CHOCH** | BOS/CHOCH visibility, swing length, and bullish/bearish colors. |

### Important control interactions

- **Show all** is a master visibility gate for several zone, legacy setup, breakout, and next-candle marker outputs. Features with their own independent controls, including MTF engulfing and BOS/CHOCH, use those controls separately.
- **Show signal markers** must be enabled for the next-candle, legacy CALL/PUT, and breakout chart labels. Engulfing markers have their own setting.
- **Use engulfing confirmation** in Price Action affects legacy rejection filtering. **Filter engulfing** in MTF Engulfing Levels controls the separate engulfing pattern definition.
- **Use confirmed HTF values** shifts the HTF fast and slow EMA series. HTF RSI is requested separately by the script.
- **Show TF levels** controls MTF Fibonacci drawings, not the watcher table or its timeframe monitoring.
- FVG, imbalance, swing-level, and previous-timeframe overlays have their own visibility controls and are independent of **Show all**.
- High-volume zones can be hidden separately with **Show high-volume boxes**; **Show high-volume box labels** controls their text independently and is on by default. The displayed value is pivot-candle volume. Their default fill is highly transparent and their default half-width is 0.20 ATR.

### Settings currently reserved or inactive

The following inputs are present in the settings interface but are not currently connected to an output or calculation in the script: **Scalp mode**, **Warn on low-quality signals**, **Show volume**, **Volume mode**, **Show composite zone**, **Show classic S/R**, **Show zigzag**, **Show strength text**, **Show tested zones**, **Show broken zones**, and **Show statistics**. The **Volume** option in Smart SnR also does not currently enable a volume-based Smart SnR calculation. These controls should not be relied on to change indicator behavior.

## Alerts

Create TradingView alerts from the indicator's alert-condition list. Available conditions are:

- **Next Candle UP**, **Next Candle DOWN**, and **Next Candle NO TRADE**. These are gated by **Enable alerts** and trigger only on confirmed chart bars when the direction changes.
- **Bullish Engulfing** and **Bearish Engulfing**. These are gated by **Enable engulfing alerts** and watch the current chart timeframe; they are not restricted to bar close by the script.
- **MP CALL Setup**, **MP PUT Setup**, **MP Bullish Breakout**, and **MP Bearish Breakout**. These legacy alert conditions are not gated by the general **Enable alerts** input.
- **New Bullish OB detected** and **New Bearish OB detected**. These are gated by **Enable OB alerts** and trigger when the order-block confirmation bar closes.

TradingView's alert frequency and delivery settings are configured separately when creating each alert. An alert must be created in TradingView; enabling an input alone does not send notifications.

## Behavior and limitations

- Pivot-based zones and BOS/CHOCH depend on confirmed swing points and therefore have confirmation delay by design.
- Order blocks are marked on their source candle only after the configured sequence and confirming bar close; the chart marker is visually offset back to that source candle. The latest channels show one bullish and one bearish block, while historical block zones remain visible.
- FVG and imbalance zones are created only after their detection candle closes. Swing-level lines use a confirmed local three-candle pattern; the separate trendline pivots use the configured pivot length and therefore confirm later.
- Previous 4H/day/week/month references use completed timeframe candles and are hidden when their interval is lower than the chart timeframe.
- The live next-candle estimate, current-chart engulfing markers, and developing higher-timeframe watcher status may change while a candle is open. Use confirmed chart-bar alerts when a close-confirmed signal is required.
- MTF engulfing intervals below the active chart timeframe are not included in the watcher or level calculation.
- The script relies on TradingView's symbol data, chart timeframe, session/timezone data, and available volume. Results may differ across data feeds and markets.
- The code currently declares a 5,000-bar history limit and 500-object limits for labels, lines, and boxes. Dense settings or long chart histories may approach platform drawing limits.
- Historical chart behavior is not a substitute for forward testing. No win rate or predictive accuracy is guaranteed by the script.

## Risk notice

This indicator provides technical analysis only. It does not place orders, account for fees or slippage, or provide financial advice. Trading involves risk, and signals should be evaluated alongside an independent trading plan and risk controls.
