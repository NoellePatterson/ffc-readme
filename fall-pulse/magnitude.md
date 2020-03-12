# Fall Pulse Flow - Magnitude

#### Definition

The fall pulse flow magnitude is set as the peak magnitude of flow during the pulse flow event. For more information on the fall pulse flow timing metric, which drives the fall pulse flow magnitude, see the section on fall pulse flow timing. This metric is measured in units of cfs. 

#### Steps

1. Determine the timing of the fall pulse flow. See the subsection on Fall Pulse Flow Timing for steps to calculate the timing of the fall pulse flow.

2. Assign the magnitude of the fall pulse flow as the peak magnitude of the fall pulse flow event.
  ```py
  elif (spl(flow_index[0] - half_duration) - min_flow) / (flow_index[1] - min_flow) < flush_threshold_perc and (spl(flow_index[0] + half_duration) - min_flow) / (flow_index[1] - min_flow) < flush_threshold_perc and flow_index[1] > broad_filter_data[int(flow_index[0])] and flow_index[1] > min_flush_magnitude and flow_index[0] <= date_cutoff:
                """both side of flow value at the peak + half duration index fall below flush_threshold_perc"""
                start_dates[-1]=int(flow_index[0])
                mags[-1]=flow_index[1]
  ```
