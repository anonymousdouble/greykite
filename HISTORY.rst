=======
History
=======

0.5.1 (2023-06-01)
------------------

Loosen dill requirements

0.5.0 (2023-04-03)
------------------

Python 3.10 support.

* New features and methods
    * Improvements on modeling holidays
        * @Yi Su: Added ``HolidayGrouper``. Holidays with a similar effect are grouped together to have fewer and more robust coefficients. With this feature, holidays are better modeled with improved forecast accuracy.
        * @Kaixu Yang: Added support for holiday neighboring impact for data with frequency longer than daily through the ``daily_event_neighbor_impact`` parameter (e.g. this enables modeling holidays on weekly data where the event dates may not fall on the exact timestamps); added holiday neighboring events (i.e. the lags of an actual holiday can be specified in the model) through the ``daily_event_shifted_effect`` parameter.
        * @Yi Su: Added holiday indicators. Now users can specify "is_event_exact", "is_event_adjacent", "is_event" (a union of both) as ``extra_pred_cols`` in the model.
    * @Reza Hosseini: Added DST indicators. Now users can specify "us_dst" or "eu_dst" in ``extra_pred_cols``. You may also use ``get_us_dst_start/end``, ``get_eu_dst_start/end`` functions to get the dates.
    * @Yi Su: Theoretical improvements for the volatility model in linear and ridge algorithm for more accurate variance estimate and prediction intervals.
    * @Phil Gaudreau: Added new evaluation metric: ``mean_interval_score``.
    * @Brian Vegetabile: Enhanced components plot that consolidates previous forecast breakdown functionality. The redesign provides a cleaner visual and allows for flexible breakdowns via regular expressions.

* Library enhancements
    * @Kaixu Yang: Python 3.10 support. Deprecated support for lower Python versions.
    * @Sayan Patra: New utility function: ``get_exploratory_plots`` to easily generate exploratory data analysis (EDA) plots in HTML.
    * @Kaixu Yang: Added ``optimize_mape`` option to quantile regression. It uses 1 over y as weights in the loss function.

* Bug fixes
    * @Qiang Fei: In case of simulation, now ``min_adimissible_value`` and ``max_adimissible_value`` will correctly cap the simulated values. Additionally, errors are propagated through simulation steps to make the intervals more accurate.
    * @Yi Su, @Sayan Patra: Now ``train_end_date`` is always respected if specified by the user. Previously it got ignored if there are trailing NA’s in training data or ``anomaly_df`` imputes the anomalous points to NA. Also, now ``train_end_date`` accepts a string value.
    * @Yi Su: The seasonality order now takes `None` without raising an error. It will be treated the same as `False` or zero.

0.4.0 (2022-07-15)
------------------

* New features and methods
    * @Reza Hosseini: Forecast interpretability. Forecasts can now be broken down to grouped components: trend, seasonality, events, autoregression, regressors, intercept, etc.
    * @Sayan Patra: Enhanced components plot. Now supports autoregression, lagged regressors, residuals; adds support for centering.
    * @Kaixu Yang: Auto model components. (1) seasonality inferrer (2) holiday inferrer (3) automatic growth.
    * @Kaixu Yang: Lag-based estimator. Supports lag-based forecasts such as week-over-week.
    * @Reza Hosseini: Fast simulation option. Provides a better accuracy and speed for mean prediction when simulation is used in autoregression.
    * @Kaixu Yang: Quantile regression option for Silverkite `fit_algorithm`.

* New model templates
    * @Kaixu Yang: AUTO. Automatically chooses templates based on the data frequency, forecast horizon and evaluation configs.
    * @Reza Hosseini, @Kaixu Yang: SILVERKITE_MONTHLY - a SimpleSilverkite template designed for monthly time series.
    * @Kaixu Yang: SILVERKITE_WOW. Uses Silverkite to model seasonality, growth and holiday effects, and then uses week-over-week to fit the residuals. The final prediction is the total of the two models.

* New datasets
    * 4 hourly datasets: Solar Power, Wind Power, Electricity, San Francisco Bay Area Traffic.
    * 1 daily dataset: Bitcoin Transactions.
    * 2 monthly datasets: Sunspot, FRED House Supply.

* Library enhancements and bug fixes
    * The SILVERKITE template has been updated to include automatic autoregression and changepoint detection.
    * Renamed `SilverkiteMultistageEstimator` to `MultistageForecastEstimator`.
    * Renamed the normalization method "min_max" to "zero_to_one".
    * @Reza Hosseini: Added normalization methods: "minus_half_to_half", "zero_at_origin".
    * @Albert Chen: Updated tutorials.
    * @Yi Su: Upgraded fbprophet 0.5 to prophet 1.0.
    * @Yi Su: Upgraded holidays to 0.13.
    * @Albert Chen @Kaixu Yang @Yi Su: Speed optimization for Silverkite algorithms.
    * @Albert Chen @Reza Hosseini @Kaixu Yang @Sayan Patra @Yi Su: Other library enhancements and bug fixes.

0.3.0 (2021-12-14)
------------------

* New tutorials
    * @Reza Hosseini: Monthly time series forecast.
    * @Yi Su: Weekly time series forecast.
    * @Albert Chen: Forecast reconciliation.
    * @Kaixu Yang: Forecast one-by-one method.
* New methods
    * @Yi Su: Lagged regressor (method was released in 0.2.0 but documentation was added in this release).
    * @Kaixu Yang @Saad Eddin Al Orjany: Model storage (method was released in 0.2.0 but documentation was added in this release).
    * @Kaixu Yang: Silverkite Multistage method for fast training on small granularity data (with tutorial).
    * @Albert Chen: Forecast reconciliation with interface and defaults optimized.
* New model templates
    * @Yi Su: `SILVERKITE_WITH_AR`: The `SILVERKITE` template with autoregression.
    * @Yi Su: `SILVERKITE_DAILY_1`: A SimpleSilverkite template designed for daily data with forecast horizon 1.
    * @Kaixu Yang: `SILVERKITE_TWO_STAGE`: A two stage model using the Silverkite Multistage method that is good for sub-daily data with a long history.
    * @Kaixu Yang: `SILVERKITE_MULTISTAGE_EMPTY`: A base template for the Silverkite Multistage method.
* Library enhancements and bug fixes
    * @Yi Su: Updated plotly to v5.
    * @Reza Hosseini: Use `explicit_pred_cols`, `drop_pred_cols` to directly specify or exclude model formula terms (see Custom Parameters).
    * @Reza Hosseini: Use `simulation_num` to specify number of simulations to use for generating forecasts and prediction intervals. Applies only if any of the lags in `autoreg_dict` are smaller than forecast_horizon (see Auto-regression).
    * @Reza Hosseini: Use `normalize_method` to normalize the design matrix (see Custom Parameters).
    * @Yi Su: Allow no CV and no backtest in pipeline.
    * @Albert Chen: Added synthetic hierarchical dataset.
    * Bug fix: `cv_use_most_recent_splits` in EvaluationPeriodParam was previously ignored.
    * @Albert Chen @Kaixu Yang @Reza Hosseini @Saad Eddin Al Orjany @Sayan Patra @Yi Su: Other library enhancements and bug fixes.

0.2.0 (2021-06-30)
------------------

* @Kaixu Yang: Removed the dependency on `fbprophet` and change it to optional.
* @Kaixu Yang @Saad Eddin Al Orjany: Added model dumping and loading for storing (see `Forecaster.dump_forecast_result` and `Forecaster.load_forecast_result`).
* @Kaixu Yang @Reza Hosseini: Added forecast one-by-one method.
* @Sayan Patra: Added the support of AutoArima by `pmdarima`, see the `AUTO_ARIMA` template.

0.1.1 (2021-05-12)
------------------

* First release on PyPI.
