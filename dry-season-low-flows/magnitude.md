# Dry Season Low Flows - Magnitude

#### Definition

The magnitude of the dry season is calculated as the 10th percentile of the flows ranging from the start date of the dry season low flow period until the start date of the wet season. This metric is measured in units of cfs. 

#### Steps

1. The timing of the start of the dry season low flow period and the start of the wet season must first be calculated, and then passed into the calculation for the dry season low flow magnitude.
  ```py
  def calc_summer_baseflow_durations_magnitude(flow_matrix, summer_start_dates, fall_flush_dates, fall_flush_wet_dates):
  ```
2. Identify the range of flow data corresponding to the start of the dry season low flow period until the start of the wet season.
  ```py
  flow_data_wet = list(flow_matrix[su_date:,column_number]) + list(flow_matrix[:wet_date, column_number])
  ```
3. Calculate the dry season low flow magnitude as the 10th percentile of flows ranging from the start of the dry season low flow period until the start of the wet season.
  ```py
  if not np.isnan(summer_start_date) and not np.isnan(fall_flush_wet_dates[column_number]):
      su_date = int(summer_start_date)
      wet_date = int(fall_flush_wet_dates[column_number])
  if not np.isnan(fall_flush_wet_dates[column_number]):
      flow_data_wet = list(flow_matrix[su_date:,column_number]) + list(flow_matrix[:wet_date, column_number])
  summer_10_magnitudes.append(np.nanpercentile(flow_data_wet, 10))
  ```
