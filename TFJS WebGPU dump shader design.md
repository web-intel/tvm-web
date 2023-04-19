Feature request: 
Dump TFJS webgpu shader (https://github.com/webatintel/tvm-web/issues/4) on Browser/Node

Solution(Please TVM team help to clarify):

1. Possible solution 1(Current enabled):  print shader to console

Turn on a flag, such as WEBGPU_PRINT_SHADER, then print the shader in the console.
```
  await tf.setBackend('webgpu');
  await tf.ready();
  tf.env().set('WEBGPU_CPU_FORWARD', false);
  tf.env().set('WEBGPU_PRINT_SHADER', 'binary');

  const benchmark = benchmarks['MobileNetV3'];
  const model = await benchmark.load();

  const predict = benchmark.predictFunc();
  await modelDemo(model, predict);
```
2. Possible solution 2: getShaders(func);

Implementation(WIP): https://github.com/tensorflow/tfjs/pull/7600

Signature:

```
/**
 * Returns shader source, an object contains shaderKey: shaderSource.
 * @param func Run tfjs ops/predict logic.
 */
 getShader(func: () => void): {[key: string]: string} {
```

Example code:
```
  await tf.setBackend('webgpu');
  await tf.ready();

  const shader = tf.backend().getShader(() => {
    {
      const a = tf.tensor1d([1, 2, 3, 4, 5, 6], 'int32');
      const b = tf.tensor1d([1, 2, 3, 4, 5, 6], 'int32');
      const c = tf.add(a, b);
    };
    {
      const a = tf.tensor1d([1, 2, 3, 4, 5, 6], 'int32');
      const b = tf.pad1d(a, [2, 3]);
    };
  });

  for (const key in shader) {
    console.group(key);
    console.debug(shader[key]);
    console.groupEnd();
  }

```
