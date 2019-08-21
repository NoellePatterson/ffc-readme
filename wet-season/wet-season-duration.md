# Wet-Season Baseflow - Duration

#### Definition

The wet season baseflow duration is defined as the number of days from the start of the wet season until the start of the next spring recession. This metric is in units of cfs. 

#### Steps

1. The timing of the start of the wet-season period and the start of the spring recession must first be calculated, and then passed into the calculation for the wet season duration.
  ```py
  def calc_fall_winter_baseflow(flow_matrix, fall_wet_timings, spring_timings):
  ```
2. Identify the range of flow data corresponding to the start of the dry season baseflow period until the start of the wet season.
  ```py
  flow_data = flow_matrix[int(fall_wet_timings[column_number]):int(spring_date), column_number]
  ```
3. Calculate the dry season baseflow duration as the number of days in the range from the start of the dry season baseflow period until the start of the wet season, identified in step 3.
  ```py
  wet_bfl_durs.append(len(flow_data))
  ```

