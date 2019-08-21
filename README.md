# Functional Flow Calculator

[![Build Status](https://travis-ci.org/leogoesger/func-flow.svg?branch=master)](https://travis-ci.org/leogoesger/func-flow)

## About

The functional flows calculator \(FFC\) quantifies key aspects of the annual flow regime based on long-term daily streamflow time series data, producing a broad suite of descriptive functional flow metrics. These metrics are meant to characterize ecologically relevant components of a flow regime in a robust, objective manner to enable comparisons of streamflow across regions, natural stream classes, and various forms and magnitudes of flow alteration. The FFC generates metrics describing aspects of streamflow timing, magnitude, duration, frequency, and rate of change, organized into five seasonally-based functional flow components: 1\) fall pulse flow, 2\) wet-season flows, 3\) peak magnitude flows, 4\) spring recession flows, and 5\) dry-season baseflow.

This project uses [Python3](https://www.python.org/) for its processing algorithm, [React](https://reactjs.org/), [Mapbox](https://www.mapbox.com/), and [D3](https://d3js.org/) for front end web development, and [Express](https://expressjs.com/), [Sequelize](http://docs.sequelizejs.com/), and [Postgres](https://www.postgresql.org/) for the server.

## Installation

For installation instructions see the
[Installation](../installation.md) section.

## To Run the Script \(Calculate Metrics\)

Run the main file in project directory:

```
   python main.py
```

## To Test

Run the test file in project directory:

```
python test/test.py
```

## To Report Errors and Bugs

Fill out the contact support form at [https://eflows.ucdavis.edu](https://eflows.ucdavis.edu)

## License

Copyright \(c\) 2018

Licensed under the [MIT license](LICENSE).
