---
layout: post
title: Simple Neural Network
category: 02_AI
tag: [tensorflow]
---

#### This is from codelab where located in [https://codelabs.developers.google.com/codelabs/neural-tensorflow-js/index.html?index=..%2F..index#0](https://codelabs.developers.google.com/codelabs/neural-tensorflow-js/index.html?index=..%2F..index#0)


<img src="/assets/images/neuralsample.jpeg"    />

#### We are going to build a simple neural network that can classify irises into three classes using TensorFlow.js:
	- Setosa
	- Virginica 
	- Versicolor



#### Getting set up
	- git clone https://github.com/aayusharora/TensorflowJS-NeuralNet.git
	- Running the project : In cmd, npm install
	- Surely, Nodejs should be installed before
	- Generate training and testing data from iris node 
	- Source code : generateTest.js



```python

let n = 144;
let k = 10;
let index = 0;
let testArray = [];
let trainArray = [];

const fs = require('fs'); 
const data = require('./iris.json');

function formDataset(data) {
// Creating training and testing dataset
//
  for(let i=0; i< Math.round(n/k); i++) {
    index = Math.floor(Math.random() * Math.round(100)) + 1;
    testArray.push(data[index]);
    data.splice(index,1) ; 
    trainArray = data;
  }  

  fs.writeFile('training.json', JSON.stringify(trainArray), function(){});
  fs.writeFile('testing.json', JSON.stringify(testArray), function(){});
}

formDataset(data);

```

#### Building Neural Network Model
	 - Refer to this git source
		- git clone https://github.com/aayusharora/TensorflowJS-NeuralNet.git
	- index.js

```python

const tf = require('@tensorflow/tfjs');
const iris = require('./training.json');
const irisTesting = require('./testing.json');

// Mapping the trainingdata
const trainingData = tf.tensor2d(iris.map(item=> [
    item.sepal_length, item.sepal_width, item.petal_length, item.petal_width
]
),[130,4])

// Mapping the testing data
const testingData = tf.tensor2d(irisTesting.map(item=> [
    item.sepal_length, item.sepal_width, item.petal_length, item.petal_width
]
),[14,4])

// creating model
const outputData = tf.tensor2d(iris.map(item => [
    item.species === 'setosa' ? 1 : 0,
    item.species === 'virginica' ? 1 : 0,
    item.species === 'versicolor' ? 1 : 0
]), [130,3])

// Creating Model
const model = tf.sequential();

model.add(tf.layers.dense(
    {   inputShape: 4, 
        activation: 'sigmoid', 
        units: 10
    }
));

model.add(tf.layers.dense(
    {
        inputShape: 10, 
        units: 3, 
        activation: 'softmax'
    }
));

model.summary();

// compiling model
model.compile({
    loss: "categoricalCrossentropy",
    optimizer: tf.train.adam()
})

async function train_data(){
    console.log('......Loss History.......');
    for(let i=0;i<40;i++){
     let res = await model.fit(trainingData, outputData, {epochs: 40});
     console.log(`Iteration ${i}: ${res.history.loss[0]}`);
  }
}


async function main() {
    await train_data();
    console.log('....Model Prediction .....')
    model.predict(testingData).print();
}

main();

```

#### Run index.js

```python
(tensorflow36gpu) C:\Users\samsung1\Desktop\TensorflowJS-NeuralNet-master\TensorflowJS-NeuralNet-master\solution>node index.js

============================

Hi there 👋. Looks like you are running TensorFlow.js in Node.js. To speed things up dramatically, install our node backend, which binds to TensorFlow C++, by running npm i @tensorflow/tfjs-node, or npm i @tensorflow/tfjs-node-gpu if you have CUDA. Then call require('@tensorflow/tfjs-node'); (-gpu suffix for CUDA) at the start of your program. Visit https://github.com/tensorflow/tfjs-node for more details.

============================


_________________________________________________________________

Layer (type)                 Output shape              Param #

=================================================================

dense_Dense1 (Dense)         [null,10]                 50

_________________________________________________________________

dense_Dense2 (Dense)         [null,3]                  33

=================================================================

Total params: 83

Trainable params: 83

Non-trainable params: 0

_________________________________________________________________

......Loss History.......

Iteration 0: 1.1662038564682007

Iteration 1: 1.0506356917894804

Iteration 2: 0.8976518456752484

Iteration 3: 0.7571693823887752

Iteration 4: 0.6467501438581027

Iteration 5: 0.5714254358640084

Iteration 6: 0.5055675928409283

Iteration 7: 0.46349796515244707

Iteration 8: 0.43163500611598676

Iteration 9: 0.3972092811877911

Iteration 10: 0.3674122438980983

Iteration 11: 0.3369893605892475

Iteration 12: 0.30642905819874544

Iteration 13: 0.27981403149091283

Iteration 14: 0.25635493993759156

Iteration 15: 0.2361850876074571

Iteration 16: 0.21609901533677028

Iteration 17: 0.20003663393167348

Iteration 18: 0.1843679586282143

Iteration 19: 0.17246765941381453

Iteration 20: 0.1598011803168517

Iteration 21: 0.15122175308374258

Iteration 22: 0.14070233599497722

Iteration 23: 0.13351650604834922

Iteration 24: 0.12579153592769915

Iteration 25: 0.11973627645235796

Iteration 26: 0.11630492943983811

Iteration 27: 0.10865777432918548

Iteration 28: 0.10432640784061872

Iteration 29: 0.10155008463905407

Iteration 30: 0.09651463008843936

Iteration 31: 0.09530227688642648

Iteration 32: 0.08995573016313406

Iteration 33: 0.0872224965920815

Iteration 34: 0.08483409400169666

Iteration 35: 0.08211586575668592

Iteration 36: 0.07996474094688892

Iteration 37: 0.07782226459911236

Iteration 38: 0.07606427617944204

Iteration 39: 0.07418118130702239

....Model Prediction .....

Tensor

    [[0.0148891, 0.0020564, 0.9830545],

     [0.9934919, 1e-7     , 0.0065079],

     [0.0003181, 0.4368047, 0.5628772],

     [0.9943109, 1e-7     , 0.005689 ],

     [0.994063 , 1e-7     , 0.0059368],

     [0.9938089, 1e-7     , 0.006191 ],

     [0.0000029, 0.9713891, 0.0286081],

     [0.9946841, 1e-7     , 0.0053159],

     [0.00013  , 0.6348923, 0.3649777],

     [0.0048211, 0.0106864, 0.9844925],

     [0.0051933, 0.0093848, 0.9854218],

     [0.0035861, 0.0175166, 0.9788972],

     [0.0039866, 0.0160286, 0.9799848],

     [0.9924375, 1e-7     , 0.0075622]]

```

> [Appendix: Simple Neural Network using TensorFlow.js from [https://codelabs.developers.google.com/codelabs/neural-tensorflow-js/index.html?index=..%2F..index#0](https://codelabs.developers.google.com/codelabs/neural-tensorflow-js/index.html?index=..%2F..index#0)












- coding

```java
import java.util.PriorityQueue;

class Student implements Comparable<Student>{
	String name;
	int age;
	
	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public int compareTo(Student o) {
		// age 가 같을 경우 name 첫번째 문자열 기준으로 내림차순
		if(this.age == o.age) return o.toString().charAt(0) - this.name.toString().charAt(0);  
		return this.age - o.age; // age 로 오름차순
	}
}


public class Solution{
	public static void main(String[] args) {
		
		PriorityQueue<Student> pq = new PriorityQueue<Student>();
		
		pq.add(new Student("Park", 23));
		pq.add(new Student("Kim", 12));
		pq.add(new Student("Ace", 66));
		pq.add(new Student("Lee", 45));
		pq.add(new Student("Hong", 44));
		pq.add(new Student("Park", 66));
		pq.add(new Student("Choi", 66));
		
		while(!pq.isEmpty()) {
			Student st = pq.poll();
			System.out.println(st.age + "\t"+ st.name);
		}
 
	}
}
```

- 출력값

```
12	Kim
23	Park
44	Hong
45	Lee
66	Ace
66	Park
66	Choi
``` 