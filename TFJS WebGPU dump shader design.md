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
2. Possible solution 2: getShaders(re, modelFunc);

Signature:
```
ShaderInfo {
shaderKey1: shaderStr1,
shaderKey2: shaderStr2,
…
}
/**
 * Returns ShaderInfo, an object of multiple shaderKey: shaderStr.
 *
 * @param re Used to match shader key.
 * @param modelFunc Run tfjs ops/predict logic.
 */
async function getShaders(re: string, modelFunc: function) : ShaderInfo
```
