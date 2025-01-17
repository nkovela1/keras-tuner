# Release v1.2.0

## Breaking changes
* If you implemented your own `Tuner`, the old use case of reporting results
  with `Oracle.update_trial()` in `Tuner.run_trial()` is deprecated. Please
  return the metrics in `Tuner.run_trial()` instead.
* If you implemented your own `Oracle` and overrided `Oracle.end_trial()`, you
  need to change the signature of the function from
  `Oracle.end_trial(trial.trial_id, trial.status)` to `Oracle.end_trial(trial)`.
* The default value of the `step` argument in `keras_tuner.HyperParameters.Int()` is
  changed to `None`, which was `1` before. No change in default behavior.
* The default value of the `sampling` argument in
  `keras_tuner.HyperParameters.Int()` is changed to `"linear"`, which was `None`
  before. No change in default behavior.
* The default value of the `sampling` argument in
  `keras_tuner.HyperParameters.Float()` is changed to `"linear"`, which was
  `None` before. No change in default behavior.
* If you explicitly rely on protobuf values, the new protobuf bug fix may affect
  you.
* Changed the mechanism of how a random sample is drawn for a hyperparameter. They
  now all start from a random value between 0 and 1, and convert the value
  to a random sample.

## New features
* A new tuner is added, `keras_tuner.GridSearch`, which can exhaust all the
  possible hyperparameter combinations.
* Better fault tolerance during the search. Added two new arguments to `Tuner`
  and `Oracle` initializers, `max_retries_per_trial` and
  `max_consecutive_failed_trials`.
* You can now mark a `Trial` as failed by
  `raise keras_tuner.FailedTrialError("error message.")` in `HyperModel.build()`,
  `HyperModel.fit()`, or your model build function.
* Provides better error messages for invalid configs for `Int` and `Float` type
  hyperparameters.
* A decorator `@keras_tuner.synchronized` is added to decorate the methods in
  `Oracle` and its subclasses to synchronize the concurrent calls to ensure
  thread safety in parallel tuning.

## Bug fixes
* Protobuf was not converting Boolean type hyperparameter correctly. This is now
  fixed.
* Hyperband was not loading the weights correctly for half-trained models. This
  is now fixed.
* `KeyError` may occur if using `hp.conditional_scope()`, or the `parent`
  argument for hyperparameters. This is now fixed.
* `num_initial_points` of the `BayesianOptimization` should defaults to `3 *
  dimension`, but it defaults to 2. This is now fixed.
* It would through an error when using a concrete Keras optimizer object to
  override the `HyperModel` compile arg. This is now fixed.
* Workers might crash due to `Oracle` reloading when running in parallel. This is
  now fixed.
