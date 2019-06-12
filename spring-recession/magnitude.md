# Spring Recession Flows - Magnitude

#### Definition

The magnitude of the spring recession is the flow magnitude on the start date of the spring recession or at the "peak" of the snowmelt pulse in snow-dominated streams. See the Spring Recession Timing section for more information on how the start date of the spring recession is calculated. This metric is in units of cfs. 

#### Steps

1. Determine the timing of the start of the spring recession. See the subsection on Spring Recession Timing for steps to calculate the timing of the start of the spring recession.

2. Assign the magnitude of the spring recession as the flow on the start date of the spring recession.
  ```py
  if len(flow_data[timings[-1] - 4: timings[-1] + 7]) > 10:
    max_flow_window_new = max(
        flow_data[timings[-1] - 4: timings[-1] + 7])
    new_timings = find_index(
        flow_data[timings[-1] - 4: timings[-1] + 7], max_flow_window_new)
    magnitudes[-1] = max_flow_window_new
  ```
