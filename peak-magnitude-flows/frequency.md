# Peak Magnitude Flows - Frequency

#### Definition

Peak flow frequency is the number of times that flow crosses over an exceedance flow threshold (10-, 5-, or 2-year recurrence intervals of annual peak flow). This metric is measured in frequency count (number of occurrences). 

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
3. In each water year, create an object each time the flow passes the exceedance value, and append that object onto an array.
  ```py
  """Init current flow object"""
        for percent in exceedance_percent:
            exceedance_object[percent] = []
            exceedance_duration[percent] = []
            current_flow_object[percent] = None

        """Loop through each flow value for the year to check if they pass exceedance threshold"""
        for row_number, flow_row in enumerate(matrix[:, column_number]):

            for percent in exceedance_percent:
                if bool(flow_row < exceedance_value[percent] and current_flow_object[percent]) or bool(row_number == len(matrix[:, column_number]) - 1 and current_flow_object[percent]):
                    """End of an object if it falls below threshold, or end of column"""
                    current_flow_object[percent].end_date = row_number + 1
                    current_flow_object[percent].get_max_magnitude()
                    exceedance_duration[percent].append(current_flow_object[percent].duration)
                    current_flow_object[percent] = None
  ```
4. Calculate the frequency by counting the length of the object array.
  ```py
  for percent in exceedance_percent:
      freq[percent].append(len(exceedance_object[percent]))
  ```
