<a id="cloud"></a>

# Cloud

OneTick Cloud provides on Demand Market Data access for Global Equities, Futures & Options.

Data Assets are divided into:

* Sample Data - Covering the first 3 months of 2024
* Consolidated Fragmented Liquidity - Combining Fragmented Liquidity from Multiple Markets
* Exchanges - Cash - Global List of Equity Exchanges
* Exchanges - Derivatives - Global List of Futures & Options Exchanges
* Indices - Global List of Index Providers
* FX & Metals - FX and Spot Metals
* Interest rates - Daily Interest Rates
* Indices - Global List of Index Providers
* OQD - One Quant Data - Security Master, Corporate Actions, Closing Prices, US ETF Constituents
* Reference - Reference Databases

Data includes:

* Tick Data - L1 Trade & Quote Data through to L3 Book Depth
* Bar Data - 1 Minute Calculuated Trade & Quote Bars
* Daily Data - Daily OHLC Data, and for Derivatives Settlement and Open Interest

<a id="sample-databases-tables"></a>

## Sample Databases & Tables

The full list of 200+ Global Equities, Futures, Options & Indices databases is available at:
\_
[OneTick Cloud Market Data Coverage](https://www.onetick.com/market-data-coverage)

The list of sample databases are additionally included below:

#### **OneTick Cloud Sample Databases**

| Database                | Description                                                   | Available Tables                        |
|-------------------------|---------------------------------------------------------------|-----------------------------------------|
| CA_COMP_SAMPLE          | Consolidated Trades & Quotes Across All Canadian Venues       | QTE, NBBO, STAT, TRD                    |
| CA_COMP_SAMPLE_BARS     | Consolidated Canadian Trade & Quote 1 Minute Bars             | QTE_1M, TRD_1M                          |
| CA_COMP_SAMPLE_DAILY    | Consolidated Canadian Trade & Quote Daily Bars                | DAY, STAT                               |
| EU_COMP_SAMPLE          | Consolidated Trades & Quotes Across All European Venues       | QTE, STAT, TRD                          |
| EU_COMP_SAMPLE_BARS     | Consolidated European Trade & Quote 1 Minute Bars             | QTE_1M, TRD_1M                          |
| EU_COMP_SAMPLE_DAILY    | Consolidated European Trade & Quote Daily Bars                | DAY, STAT                               |
| LSE_SAMPLE              | London Stock Exchange Trades, Quotes & Book Depth             | DAY, IND, MKT, PRL_FULL, QTE, STAT, TRD |
| LSE_SAMPLE_BARS         | LSE Trade & Quote 1 Minute Bars                               | QTE_1M, TRD_1M, DAY                     |
| LSE_SAMPLE_DAILY        | LSE Daily Bars                                                | DAY, STAT                               |
| TDI_FUT_SAMPLE          | Global Futures  Trades & Quotes                               | QTE, STAT, TRD                          |
| TDI_FUT_SAMPLE_BARS     | Global Futures Trades & Quote 1 Minute Bars                   | QTE_1M, TRD_1M                          |
| TDI_FUT_SAMPLE_DAILY    | Global Futures Daily Bars                                     | DAY, STAT                               |
| US_COMP_SAMPLE          | Consolidated Trades & Quotes Across All US Venues             | QTE, STAT, TRD                          |
| US_COMP_SAMPLE_BARS     | Consolidated US Trade & NBBO 1 Minute bars                    | QTE_1M, TRD_1M                          |
| US_COMP_SAMPLE_DAILY    | Consolidated US Daily Bars                                    | DAY, STAT                               |
| CME_SAMPLE              | CME Futures  Trades & Quotes                                  | QTE, STAT, TRD                          |
| CME_SAMPLE_BARS         | CME Futures Trades & Quote 1 Minute Bars                      | QTE_1M, TRD_1M                          |
| CME_SAMPLE_DAILY        | CME Futures Daily Bars                                        | DAY, STAT                               |
| EUREX_SAMPLE            | EUREX Futures  Trades & Quotes                                | QTE, STAT, TRD                          |
| EUREX_SAMPLE_BARS       | EUREX Futures Trades & Quote 1 Minute Bars                    | QTE_1M, TRD_1M                          |
| EUREX_SAMPLE_DAILY      | EUREX Futures Daily Bars                                      | DAY, STAT                               |
| ICE_EU_COM_SAMPLE       | ICE European Commodities Futures  Trades & Quotes             | QTE, STAT, TRD                          |
| ICE_EU_COM_SAMPLE_BARS  | ICE European Commodities Futures Trades & Quote 1 Minute Bars | QTE_1M, TRD_1M                          |
| ICE_EU_COM_SAMPLE_DAILY | ICE European Commodities Futures Daily Bars                   | DAY, STAT                               |
| ICE_US_SAMPLE           | ICE US Futures  Trades & Quotes                               | QTE, STAT, TRD                          |
| ICE_US_SAMPLE_BARS      | ICE US Futures Trades & Quote 1 Minute Bars                   | QTE_1M, TRD_1M                          |
| ICE_US_SAMPLE_DAILY     | ICE US Futures Daily Bars                                     | DAY, STAT                               |
| GLOBAL_FX_SAMPLE        | Global FX Spot Quotes                                         | QTE, STAT                               |
| GLOBAL_FX_SAMPLE_BARS   | Global FX Spot Quote 1 Minute Bars                            | QTE_1M                                  |
| GLOBAL_FX_SAMPLE_DAILY  | Global FX Spot Quote Daily Bars                               | DAY, STAT                               |
| US_OPTIONS_SAMPLE       | OPRA US Options Trades, Quotes & NBBO                         | QTE, STAT, TRD, NBBO                    |
| US_OPTIONS_EOD_SAMPLE   | OPRA US Options Daily Bars & Greeks                           | DAY, STAT                               |
| SYMBOL_UNIVERSE         | Symbol Universe across all available Venues                   | STAT                                    |
| DB_INFO                 | Venue Database availability times                             | PROC_EVENTS                             |
| OQD_MKT_CAL             | Market Holidays & Trading Hours                               | MKTCAL                                  |

<a id="id1"></a>

### \_

Data is stored in standardized tables

#### **OneTick Cloud Standard Tables**

| Table    | Description                                                                                                                       |
|----------|-----------------------------------------------------------------------------------------------------------------------------------|
| DAY      | End of Day Record typically covering OHLC Prices, plus Volume Splits and Settlement Price & Open Interest for Derivatives Markets |
| IND      | Indicative Prices occuring during Auction phases                                                                                  |
| QTE      | Quote Events                                                                                                                      |
| STAT     | Static Reference Data for the Instrument                                                                                          |
| TRD      | Trade Events                                                                                                                      |
| NBBO     | National Best Bid & Offer Quotes                                                                                                  |
| PRL      | Book Depth - Market By Level                                                                                                      |
| PRL_FULL | Book Depth - Market by Order                                                                                                      |
| MKTCAL   | Market Holiday & Trading Hours                                                                                                    |
| TRD_1M   | 1 Minute Trade Bar                                                                                                                |
| QTE_1M   | 1 Minute Quote Bar                                                                                                                |
