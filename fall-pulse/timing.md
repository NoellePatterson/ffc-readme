# Wet Season Initiation Flows - Timing

#### Definition

The wet season initiation timing captures the date of the first storm event of the new water year. This is meant to characterize the first significant increase in flows following the dry season. The wet season initiation event is the first date between October 1st to December 15th in which flow exceeds the initiation event threshold, which is defined as twice the magnitude of the previous dry season’s base flow or 1 cfs, whichever is greater. This metric is measured in Julian days, where January 1st = 1 and December 31st = 365.

#### Steps

1. Convert raw date and flow data columns into a flow matrix based on water year. Each column starts with the beginning of the water year \(i.e. 10/1\) and ends with the end of the water year \(i.e.9/30\).

1. Check if the data contains more than allowed `None` and `zero` data. Then, interpolate between missing values to get a full dataset:

   ```py
   if np.isnan(flow_matrix[:, column_number]).sum() > max_nan_allowed_per_year or np.count_nonzero(flow_matrix[:, column_number]==0) > max_zero_allowed_per_year:
            continue
   flow_data = replace_nan(flow_data)
   ```

2. Identify the start date of the peak magnitude season. For details on finding the timing of the wet season, see the steps for peak magnitude start timing calculation in the peak magnitude flows section.

3. Filter the entire water year's flow data with a small filter, and fit a cubic spline function to the curve.

   ```py
   """Filter noise data with small sigma to find fall flush hump"""
        filter_data = gaussian_filter1d(flow_data, sigma)
   """Fit spline"""
        x_axis = list(range(len(filter_data)))
        spl = ip.UnivariateSpline(x_axis, filter_data, k=3, s=3)
   ```

4. Using the peak detection function on the spline curve, find the peaks and valleys across the entire water year.

   ```py
   """Find the peaks and valleys of the filtered data"""
        mean_flow = np.nanmean(filter_data)
        maxarray, minarray = peakdet(spl(x_axis), mean_flow * peak_sensitivity)
   ```

5. Loop through the peaks in the data and identify the initiation event as the peak that fills the following requirements:

   * Timing is before wet season initiation or before December 15th, whichever is earlier
   * Duration of the rising limb of the initiation event is under 20 days \(ensures the peak is flashy enough\)
   * The peak itself, from bottom of the rising limb on either side to the top of the peak, is above a relative percent of 30%. This also ensures that the peak is flashy enough.
   * Relative magnitude is above the median baseflow magnitude threshold. The value of this threshold varies depending on the magnitude of the baseflow. The threshold is usually set as 2 times the previous dry season's baseflow, unless the baseflow is exceptionally high \(above 25 cfs\), in which case the threshold is set as 1.5 times the baseflow. The minimum threshold allowed, regardless of median baseflow, is 3cfs by default:.
     ```py
     if bs_med > 25:
            min_flush_magnitude = bs_med * 1.5 # if median baseflow is large (>25), magnitude threshold is 50% above median baseflow of previous summer
        else:
            min_flush_magnitude = bs_med * 2 # otherwise magnitude threshold is 100% above median baseflow of previous summer
        if min_flush_magnitude < min_flush_threshold:
            min_flush_magnitude = min_flush_threshold
     ```

6. The first peak in the data to fill the above requirements is the wet season initiation event. The timing of the wet season initiation is at the peak flow of the event. If no peak fulfills the above requirements, then there is no wet season initiation recorded for that water year.
