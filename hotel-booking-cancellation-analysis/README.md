# Hotel Booking Cancellation Analysis

A Power BI report on 118,985 bookings at two hotels in Portugal, a city hotel in Lisbon and a resort hotel in the Algarve, covering arrivals from July 2015 to August 2017. It answers one business question: **who cancels, and what does it cost?**

Built with Power BI Desktop, Power Query and DAX.

## Files in this repository

| File | What it is |
|---|---|
| `hotel-booking-cancellation-analysis.pbix` | The Power BI report, including the data model, measures and Power Query steps. Open it in Power BI Desktop (free) to use the Hotel and Date slicers. |
| `hotel-booking-cancellation-analysis.pdf` | A static export of all four pages, for a quick look without Power BI. |
| `README.md` | This file. |

## Key findings

Over the period, 37.1% of bookings were cancelled. The cancelled bookings were worth €16.69M, or 39.1% of all booked value, compared with €25.95M of room revenue from guests who stayed.

| Finding | Figure |
|---|---|
| City Hotel cancels more often than the Resort | 41.8% vs 27.8% |
| Bookings made more than 6 months ahead cancel most | 57%, vs 10% for bookings made within a week |
| Portuguese bookings cancel more than twice as often as other markets | 57% vs 24% |
| Group bookings cancel more than any other segment but bring in little revenue | 61% cancelled, 7% of room revenue |
| Online travel agents bring in the most revenue | 53% of room revenue |
| Guests with at least one special request cancel less than half as often | 22% vs 48% |
| Repeat guests cancel far less than new guests | 15% vs 38% |
| Cancelled value rose sharply in 2017 | Jan–Aug 2017 was 53% above the same months of 2016 |
| Like-for-like growth (same months, year on year) | Bookings +19.4%, room revenue +35.1% |

One result is flagged rather than used as a finding: non-refundable bookings show a 99% cancellation rate, and 97% of them are Portuguese. That is the opposite of what a deposit should do, and it may reflect how the hotels recorded group or block bookings. It needs checking with the hotels before drawing conclusions.

## Report pages

| Page | What it covers |
|---|---|
| 1. Overview | How are the hotels performing? Headline KPIs, bookings and average daily rate by month, room revenue vs cancelled value, cancelled value by month. |
| 2. Cancellations | Who cancels, and what does it cost? Cancellation rate by country, hotel, lead time, deposit type, market segment and special requests. |
| 3. Guests & Rooms | Who are the guests? Guest countries, meal plans, length of stay, new vs repeat guests, room type booked vs assigned, room revenue by market segment. |
| 4. About this report | Data source, definitions, cleaning steps and things to keep in mind. |

## Data source

The data is not mine. It was collected and published by the original authors, and you can download it from any of these:

| Source | Link |
|---|---|
| Original paper | Antonio, N., de Almeida, A. & Nunes, L. (2019). *Hotel booking demand datasets.* Data in Brief, 22, 41–49. https://doi.org/10.1016/j.dib.2018.11.126 |
| TidyTuesday (the version used in this report) | https://github.com/rfordatascience/tidytuesday/tree/master/data/2020/2020-02-11 (file: `hotels.csv`) |
| Kaggle | https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand |

If you use the data, please cite the paper above.

The raw CSV is not included in this repository. The `.pbix` file already contains the imported data, so it opens without it. To refresh the report from the source, download `hotels.csv` from TidyTuesday, open the `.pbix`, go to **Transform data → Data source settings → Change Source**, point it to your copy, and click **Refresh**.

## Data cleaning

All cleaning was done in Power Query; the raw file was never edited. The raw data has 119,390 bookings, and 118,985 remain after cleaning.

| Step | Rows |
|---|---|
| Removed bookings with no adults (180 with no guests at all, 223 with only children or babies) | 403 |
| Removed invalid daily rates (one negative, −6.38, and one extreme outlier, 5,400) | 2 |
| Replaced missing child counts ("NA") with 0 | 4 |
| Merged "Undefined" meal plans into Self Catering, as the data documentation says they mean the same thing | 1,169 |
| Replaced "NULL" agent and company with "No Agent" and "No Company" (NULL here means not applicable, not missing) | 16,340 and 112,593 |
| Marked missing countries as Unknown | 488 |
| Mapped country code CN to Canada (an assumption: CN is not a valid three-letter code, China already appears as CHN, and Canada's code never appears) | 1,279 |
| Mapped the old code TMP to Timor-Leste | 3 |
| Replaced "Undefined" market segment and distribution channel with Unknown | 2 and 5 |
| Kept duplicate rows (the data has no booking ID, so identical rows may be separate bookings, such as several rooms in one group booking) | 31,976 |
| Kept zero-night bookings (they add no room nights or revenue, so they don't affect the average daily rate) | 645 |

## Data model

The model has three tables. **Bookings** holds one row per booking, with calculated columns such as total nights, total guests, lead time bands and a room-changed flag. **Date** is a DAX calendar table for 2015–2017, related to the bookings' arrival date, with a Month Start column for the monthly charts. **_Measures** holds all the measures.

Revenue measures count only bookings with the status Check-Out. The average daily rate is room revenue divided by room nights, as in standard hotel reporting. The year-on-year measures compare only months that have data in both years, because 2015 and 2017 are partial years.

## Limitations

2015 (July to December) and 2017 (January to August) are partial years, so full-year totals shouldn't be compared directly; the report compares the same months instead. Cancelled booking value is what the cancelled bookings were worth, not confirmed lost revenue, because some rooms were probably resold. Room types are anonymised letter codes, so room-level findings can't be linked to real room names. The CN-to-Canada mapping is an assumption, and duplicate rows can't be confirmed as real or accidental without a booking ID.

## Checking the data against the source

Before cleaning, the data was checked against the original paper by Antonio, de Almeida and Nunes (2019). It matches: 119,390 bookings in total, 40,060 at the resort hotel and 79,330 at the city hotel, with arrivals from 1 July 2015 to 31 August 2017.

## Use of AI

I used Claude (an AI assistant by Anthropic) as a reviewer while finishing the report. It suggested fixes for chart setup and formatting, the DAX for the like-for-like year-on-year measures, and tighter wording for chart titles and the About page.
