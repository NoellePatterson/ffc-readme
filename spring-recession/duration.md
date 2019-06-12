# Spring Recession Flows - Duration

#### Definition

The duration of the spring recession is the period of elapsed time from the start date of the spring recession until the start date of the following dry season low flows period. This metric is measured in number of days.

#### Steps

1. The timing of the start of the spring recession and the start of the dry season low flows period must first be calculated, and then passed into the calculation for the spring recession duration.
  ```py
  def calc_spring_transition_duration(spring_timings, summer_timings):
  ```
2. If both timing metrics are available for the water year, then the spring recession duration is calculated as the number of days from the start of the spring recession to the start of the dry season low flows period.
  ```py
  for index, spring_timing in enumerate(spring_timings):
          if spring_timing and summer_timings[index] and summer_timings[index] > spring_timing:
              duration_array.append(summer_timings[index] - spring_timing)
          else:
              duration_array.append(None)
  ```
