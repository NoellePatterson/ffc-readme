# Wet-Season Baseflow - Magnitude

#### Definition

The baseflow magnitude of wet season is defined as 10th or 50th percentile magnitude of daily flow from the start of the wet season to the start of the dry season. This metric is in units of cfs. 

#### Steps

1. Convert raw date and flow data columns into a flow matrix based on water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of the water year \(i.e.9/30\).

2. The timing for the start of the peak magnitude season and the start of the dry season must first be calculated, and then passed into the calculation for the wet season magnitude baseflow.
  ```py
  def calc_fall_winter_baseflow(flow_matrix, fall_wet_timings, summer_timings):
  ```
3. Identify the range of days in the annual flow matrix from the start of the dry season until the start of the peak magnitude season.
  ```py
  if summer_date and fall_wet_timings[column_number] and not np.isnan(summer_date) and not np.isnan(fall_wet_timings[column_number]):
      if fall_wet_timings[column_number] and summer_date > fall_wet_timings[column_number]:
          flow_data = flow_matrix[int(fall_wet_timings[column_number]):int(summer_date), column_number]
      else:
          flow_data =[]
                ```
4. Calculate the 10th and 50th percentile of flows in the range found in step 3. Report this value as the peak magnitude season baseflow magnitude.
  ```py
  if flow_data:
      wet_baseflows_10.append(np.nanpercentile(flow_data, 10))
      wet_baseflows_50.append(np.nanpercentile(flow_data, 50))
  else:
      wet_baseflows_10.append(None)
      wet_baseflows_50.append(None)
  ```
