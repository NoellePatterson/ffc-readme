# Peak Magnitude Flows - Magnitude

#### Definition

This metric is the magnitude of the exceedance flow threshold for the 10-, 5-, or 2-year recurrence intervals of annual peak flow. This metric is in units of cfs.

#### Steps

1. Convert raw data from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 10%, 20%, and 50% exeedance values of annual peak flow over the entire period of record.
  ```py
  for column_number, _ in enumerate(matrix[0]):
        flow_data = matrix[:, column_number]
        peak_flows.append(np.nanmax(flow_data))
    for percentile in peak_percentiles:
        peak_exceedance_values.append(np.nanpercentile(peak_flows, 100 - percentile))
  ```
3. Set the exceedance flow value as the final value for the magnitude metric.
  ```py
  for percent in exceedance_percent:
            magnitude[percent].append(exceedance_value[percent])
  ```
