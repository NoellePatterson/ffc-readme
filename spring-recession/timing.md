# Spring Recession Flows - Timing

#### Definition

The timing of the start of the spring recession is identified as the point in which an overall decrease in flow occurs, following the water year's high flows during winter. The start date of the spring recession period is meant to capture the beginning of the period during which winter baseflow gradually recedes down to summer baseflow. This metric is measured in Julian days, where January 1st = 1 and December 31st = 365.

#### Steps

1. Convert raw date and flow data columns into a flow matrix based on water year. Each column starts with the beginning of the water year \(e.g. 10/1\) and ends with the end of the water year \(e.g. 9/30\).

2. Check if the data contains more than the maximum allowed `None` and `zero` data:

   ```py
   if np.isnan(flow_matrix[:, column_number]).sum() > max_nan_allowed_per_year or np.count_nonzero(flow_matrix[:, column_number]==0) > max_zero_allowed_per_year:
            continue
   ```

3. Apply a heavy Gaussian filter to smooth the raw flow input:

   ```py
   filter_data = gaussian_filter1d(flow_data, window_sigma)
   ```

4. Find the peaks and valleys of the filtered data, and use these to identify the last \(rightmost\) major peak event.

   ```py
   mean_flow = np.nanmean(filter_data)
   maxarray, minarray = peakdet(filter_data, mean_flow * peak_sensitivity)

   for counter, flow_index in enumerate(reversed(maxarray)):
        if int(flow_index[0]) < max_peak_flow_date and (flow_index[1] - min_flow) / flow_range > peak_filter_percentage:
             max_flow_index = int(flow_index[0])
             break
   ```

5. Set a search window \(default 20 days before and 50 days after\) around the identified peak event and apply a smaller Gaussian filter (less smoothing) to the windowed data.

   ```py
   x_axis_window = list(range(max_flow_index - search_window_left, max_flow_index + search_window_right))
   flow_data_window = gaussian_filter1d(flow_data[max_flow_index - search_window_left : max_flow_index + search_window_right], fit_sigma)
   ```

6. Fit a spline curve to the filtered data in this region, then calculate the first derivative of the spline and find all points where the derivative crosses zero. This identifies an array of local peaks based on timing of the sign change from positive to negative.

   ```py
   spl = ip.UnivariateSpline(x_axis_window, flow_data_window, k=3, s=3)
   spl_first_deriv = spl.derivative(1)
   index_zeros = crossings_nonzero_all(spl_first_deriv(x_axis_window))
   ```

7. Loop through the array of peaks found in step 6 to identify the last \(rightmost\) peak which fills the following requirements:

   * The 4 preceding days are increasing continuously, with a certain degree of increase.
   * The relative flow \(Q-Qmin\) magnitude must be at least 50% of peak relative flow magnitude \(Qmax-Qmin\). Qmin and Qmax are calculated as the smoothed minimum and maximum flows within the search window.
   * Timing occurs before a user-defined cutoff set near the end of the water year, and timing is set before that year's dry season start timing. 

   ```py
   for i in reversed(new_index):
      threshold = max(spl_first_deriv(x_axis_window))
      max_flow_window = max(spl(x_axis_window))
      min_flow_window = min(spl(x_axis_window))
      range_window = max_flow_window - min_flow_window

      if summer_timings[column_number] is not None and i < summer_timings[column_number] and i > timing_cutoff and spl(i) - spl(i-1) > threshold * current_sensitivity * 1 and spl(i-1) - spl(i-2) > threshold * current_sensitivity * 2 and spl(i-2) - spl(i-3) > threshold * current_sensitivity * 3 and spl(i-3) - spl(i-4) > threshold * current_sensitivity * 4 and (spl(i) - min_flow_window) / range_window > min_percentage_of_max_flow:
         timings[-1] = i;
         break;
   ```

8. Search the unfiltered data 4 days before and 7 days after the identified start date. Change the start of spring date to the highest flow date within this range.
   ```py
   if len(flow_data[timings[-1] - 4 : timings[-1] + 7]) > 10:
      max_flow_window_new = max(flow_data[timings[-1] - 4 : timings[-1] + 7])
      new_timings = find_index(flow_data[timings[-1] - 4 : timings[-1] + 7], max_flow_window_new)
      timings[-1] = timings[-1] - 4 + new_timings + lag_time
      magnitudes[-1] = max_flow_window_new
   ```
9. Finally, shift the start date 4 days after the date just identified. This is meant to eliminate the flow effects of isolated storms as much as possible.
