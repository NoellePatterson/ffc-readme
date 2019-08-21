# Dry Season Baseflow - Timing

#### Definition

The timing of the dry season baseflow captures the date in which the high flows of the winter season have receded sufficiently into the low flow dry season period. This metric is measured in Julian days, where January 1st = 1 and December 31st = 365.

#### Steps

1. Assemble data into a water year matrix, and append each column of water year data with the first 30 days of the following water year. This is done for cases in which the dry season occurs later than September 31st, which is the end of the California water year. This is most common in snowmelt-sourced flow regimes. 
   ```py
   """Append each column with 30 more days from next column, except the last column"""
        if column_number != len(matrix[0])-1:
            flow_data = list(matrix[:,column_number]) + list(matrix[:100,column_number+1])
        else:
            flow_data = matrix[:, column_number]
   ```
2. Prepare the flow data for analysis by interpolating between blank values, smoothing the timeseries with a large-sigma Gaussian filter, and fitting a spline curve to the smoothed data.

    ```py
    flow_data = replace_nan(flow_data)
    smooth_data = gaussian_filter1d(flow_data, sigma)
    spl = ip.UnivariateSpline(x_axis, smooth_data, k=3, s=3)
    spl_first = spl.derivative(1)
    ```

3. From the smoothed data, identify the last major peak of the hydrograph. This is meant to represent the last significant flow event of the wet season, or the last "critical mass" of flow. A user-defined late-season threshold is set \(default Julian Day 325\), so that the last major season peak flow cannot occur beyond this peak. This is to ensure that the identified peak captures the true wet season, instead of an unseasonal storm flow in the summer period, which sometimes occurs in both storm- and snowmelt- driven streams.

   ```py
   """Find the major peaks of the filtered data"""
          mean_flow = np.nanmean(flow_data)
          maxarray, minarray = peakdet(smooth_data, mean_flow * peak_sensitivity)
          """Set search range after last smoothed peak flow"""
          for flow_index in reversed(maxarray):
              if int(flow_index[0]) < max_peak_flow_date:
                  max_flow_index = int(flow_index[0])
                  break
   ```

4. Set a magnitude threshold, under which the start of summer can begin. The magnitude threshold is calculated as a relative magnitude above the minimum magnitude of the smoothed data. The relative magnitude is a relative value based on both the minimum and maximum smoothed flow values.

   ```py
   """Set a magnitude threshold below which start of summer can begin"""
          min_flow_data = min(smooth_data[max_flow_index:366])
          threshold = min_flow_data + (smooth_data[max_flow_index] - min_flow_data) * min_summer_flow_percent
   ```

5. Search the flow data from left to right, starting at the last major peak of flow found in step 3. Check the flow in each date for the first date that fills the following requirements for the start date of the dry season:
   *  The flow must be under the flow magnitude threshold set in step 4
   * The rate of change must be below a set threshold for the rate of change.

   The first \(leftmost\) date that fulfills these requirements is set as the start date of the dry season.

    ```py
     """Search criteria: derivative is under rate of change threshold, date is after last major peak, and flow is less than specified percent of smoothed max flow"""
              if abs(spl_first(index)) < max_flow_data * current_sensitivity and index > max_flow_index and data < threshold:
                  start_dates[-1] = index
                  break
     ```
