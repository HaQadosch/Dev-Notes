```javascript
describe('clipReducer/setClipStopTime', async assert => {
  const stopTime = 5;
  const clipState = {
    startTime: 2,
    stopTime: Infinity
  };

  assert({
    given: 'clip stop time',
    should: 'set clip stop time in state',
    actual: clipReducer(clipState, setClipStopTime(stopTime)),
    expected: { ...clipState, stopTime }
  });
});
```