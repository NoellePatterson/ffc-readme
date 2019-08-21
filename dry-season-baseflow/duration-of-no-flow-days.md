# Dry Season Baseflow - Duration \(\# of no flow days\)

#### Description

The duration of no-flow days is the number of days during the dry season in which the flow is 0. The metric is measured in number of days.

#### Steps

1. The timing of the start of the dry season baseflow period and the start of the wet season must first be calculated, and then passed into the calculation for the dry season baseflow duration.
  ```py
  def calc_summer_baseflow_durations_magnitude(flow_matrix, summer_start_dates, fall_flush_dates, fall_flush_wet_dates):
  ```
2. Identify the range of flow data corresponding to the start of the dry season baseflow period until the start of the wet season.
  ```py
  flow_data_wet = list(flow_matrix[su_date:,column_number]) + list(flow_matrix[:wet_date, column_number])
  ```
3. Calculate the duration of no-flow days as the total number of days in the range from the start of the dry season low flow period until the start of the wet season in which the flow magnitude is equal to zero.
  ```py
  summer_no_flow_counts.append(len(flow_data_wet) - np.count_nonzero(flow_data_wet))
  ```
