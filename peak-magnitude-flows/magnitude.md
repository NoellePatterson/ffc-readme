# Peak Magnitude Flows - Magnitude

#### Definition

This metric is the magnitude of the exceedance flow threshold for the 2nd, 5th, 10th, and 20th percentile exceedance flows over the period of record. This metric is in units of cfs.

#### Steps

1. Convert raw data of from a single column of dates and a single column of flows into a matrix with columns organized by water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of water year \(9/30\).
2. Calculate the 2nd, 5th, 10th, and 20th percentile exceedance flow values over the entire period of record \(i.e., the entire flow matrix\).
  ```py
  for i in exceedance_percent:
        exceedance_value[i] = np.nanpercentile(matrix, 100 - i)
  ```
3. Set the exceedance flow value as the final value for the magnitude metric.
  ```py
  for percent in exceedance_percent:
            magnitude[percent].append(exceedance_value[percent])
  ```
