# Wet Season Initiation Flow - Magnitude

#### Definition

The wet season initiation event magnitude is set as the peak magnitude of flow during the initiation flow event. For more information on the initiation event timing metric, which drives the initiation event magnitude, see the section on wet season initiation timing. This metric is measured in units of cfs. 

#### Steps

1. Determine the timing of the wet season initiation event. See the subsection on Wet Season Initiation Timing for steps to calculate the timing of the wet season initiation.

2. Assign the magnitude of the wet season initiation flow as the flow on the start date of the spring recession.
  ```py
  elif (spl(flow_index[0] - half_duration) - min_flow) / (flow_index[1] - min_flow) < flush_threshold_perc and (spl(flow_index[0] + half_duration) - min_flow) / (flow_index[1] - min_flow) < flush_threshold_perc and flow_index[1] > broad_filter_data[int(flow_index[0])] and flow_index[1] > min_flush_magnitude and flow_index[0] <= date_cutoff:
                """both side of flow value at the peak + half duration index fall below flush_threshold_perc"""
                start_dates[-1]=int(flow_index[0])
                mags[-1]=flow_index[1]
  ```
