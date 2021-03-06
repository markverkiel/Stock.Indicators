﻿# On-Balance Volume

[On-balance Volume](https://en.wikipedia.org/wiki/On-balance_volume) is a rolling accumulation of volume based on Close price direction.

![image](chart.png)

```csharp
// usage
IEnumerable<ObvResult> results = Indicator.GetObv(history);

// usage with optional overlay SMA of OBV (shown above)
IEnumerable<AdlResult> results = Indicator.GetObv(history,smaPeriod);  
```

## Parameters

| name | type | notes
| -- |-- |--
| `history` | IEnumerable\<[TQuote](../../docs/GUIDE.md#quote)\> | Historical Quotes data should be at any consistent frequency (day, hour, minute, etc).  You must supply at least two historical quotes; however, since this is a trendline, more is recommended.
| `smaPeriod` | int | Optional.  Number of periods (`N`) in the moving average of OBV.  Must be greater than 0, if specified.

## Response

```csharp
IEnumerable<ObvResult>
```

The first period OBV will have `0` value since there's not enough data to calculate.  We always return the same number of elements as there are in the historical quotes.

### ObvResult

| name | type | notes
| -- |-- |--
| `Date` | DateTime | Date
| `Obv` | decimal | On-balance Volume
| `Sma` | decimal | SMA of the OBV based on `smaPeriod` periods, if specified

**Warning**: absolute values in OBV are somewhat meaningless, so use with caution.

## Example

```csharp
// fetch historical quotes from your favorite feed, in Quote format
IEnumerable<Quote> history = GetHistoryFromFeed("SPY");

// calculate
IEnumerable<ObvResult> results = Indicator.GetObv(history);

// use results as needed
ObvResult result = results.LastOrDefault();
Console.WriteLine("OBV on {0} was {1}", result.Date, result.Obv);
```

```bash
OBV on 12/31/2018 was 539843504
```
