# Numenta Unicorn: Backend Model Runner

This directory is for ModelRunner and Support (Python / C++).
It's functionality is exposed to the user via GUI and Model Management code.

See: `requirements.txt`


## Example usage
获得model 参数
python param_finder_runner.py --input '{"csv":"../data/cpu.csv","datetimeFormat":"%Y-%m-%d %H:%M:%S","timestampIndex":0,"rowOffset":0,"valueIndex":1}'

根据获得的参数探测异常数据：
python  unicorn_backend/model_runner_2.py --input '{"csv":"data/cpu.csv","datetimeFormat":"%Y-%m-%d %H:%M:%S","timestampIndex":0,"rowOffset":0,"valueIndex":1}' --model '{"timestampFieldName":"c0","valueFieldName":"c1","inferenceArgs":{"predictionSteps": [1], "predictedField": "c1", "inputPredictedField":"auto"},"modelId":"1","modelConfig":{"aggregationInfo": {"seconds": 0, "fields": [], "months": 0, "days": 0, "years": 0, "hours": 0, "microseconds": 0, "weeks": 0, "minutes": 0, "milliseconds": 0}, "model": "CLA", "version": 1, "predictAheadTime": null, "modelParams": {"sensorParams": {"sensorAutoReset": null, "encoders": {"c0_dayOfWeek": null, "c0_timeOfDay": null, "c1": {"name": "c1", "resolution": 1.0757743589743589, "seed": 42, "fieldname": "c1", "type": "RandomDistributedScalarEncoder"}, "c0_weekend": null}, "verbosity": 0}, "anomalyParams": {"anomalyCacheRecords": null, "autoDetectThreshold": null, "autoDetectWaitRecords": 5030}, "spParams": {"columnCount": 2048, "synPermInactiveDec": 0.0005, "spatialImp": "cpp", "inputWidth": 0, "spVerbosity": 0, "synPermConnected": 0.2, "synPermActiveInc": 0.003, "potentialPct": 0.8, "numActiveColumnsPerInhArea": 40, "boostStrength": 0.0, "globalInhibition": 1, "seed": 1956}, "trainSPNetOnlyIfRequested": false, "clParams": {"alpha": 0.035828933612158, "verbosity": 0, "steps": "1", "regionName": "SDRClassifierRegion"}, "tpParams": {"columnCount": 2048, "activationThreshold": 13, "pamLength": 3, "cellsPerColumn": 32, "permanenceDec": 0.1, "minThreshold": 10, "inputWidth": 2048, "maxSynapsesPerSegment": 32, "outputType": "normal", "initialPerm": 0.21, "globalDecay": 0.0, "maxAge": 0, "newSynapseCount": 20, "maxSegmentsPerCell": 128, "permanenceInc": 0.1, "temporalImp": "cpp", "seed": 1960, "verbosity": 0}, "clEnable": false, "spEnable": true, "inferenceType": "TemporalAnomaly", "tpEnable": true}}}'




To generate mock data and feed it to the model runner, run:

```
python unicorn_backend/mock_data_generator.py | \
python unicorn_backend/model_runner.py \
  --model "1" \
  --stats '{"max": 10, "min": 0}'
```

This will output the model output to stdout.

Optionally, you may override the parameters in the model by way of the
`--replaceParam` CLI argument.  For example:

```
python unicorn_backend/model_runner.py \
  --model "1" \
  --stats '{"max": 10, "min": 0}' \
  --replaceParam modelConfig/modelParams/spParams/spVerbosity 1
```

See `python unicorn_backend/model_runner.py --help` for full details on
command line options.

Note that simply running `python unicorn_backend/mock_data_generator.py` will
just generate the data input. When you pipe the two scripts together
(`mock_data_generator.py` to `model_runner.py`) this will feed the data input
to the model runner via stdout and in turn, it outputs the model results to
stdout.

## Tests

- Run unit AND integration tests

  ```
  python setup.py test
  ```

- Run unit tests only

  ```
  python setup.py test -a unit
  ```

- Run integration tests only

  ```
  python setup.py test -a integration
  ```

- Run a specific test

  ```
  python setup.py test -a "-k <matching term>"
  ```

- Help on additional functionality

  ```
  python setup.py test -a "--help"
  ```
