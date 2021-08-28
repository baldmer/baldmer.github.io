---
layout: post
title: Hopfield Neural Network, C++ source code to recall binary patterns
tags: [Data-Science, Neural-Networks, C++, OOP]
---

The g++ version I have used to code this Network is:

```
$ g++ --version 
g++ (Debian 4.3.2-1.1) 4.3.2 
Copyright (C) 2008 Free Software Foundation, Inc. 
This is free software; see the source for copying conditions.  There is NO 
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. 
```

Compiling the Network

```
$ g++ main.cpp -o hopfield 
```

Running the Network

```
$ ./hopfield
*******************************
Hopfield Neural Network
able to recall binary patterns
*******************************
Presenting pattern A
 component = 1 output = 1 component matched
 component = 0 output = 0 component matched
 component = 1 output = 1 component matched
 component = 0 output = 0 component matched
* Pattern A recalled correctly
Presenting pattern B
 component = 0 output = 0 component matched
 component = 1 output = 1 component matched
 component = 0 output = 0 component matched
 component = 1 output = 1 component matched
* Pattern B recalled correctly
```

Try to modify the input patterns, for example by presenting a pattern like B = {0, 1, 0, 0}, the output would be like this:

```
.
.
Presenting pattern B
 component = 0 output = 0 component matched
 component = 1 output = 1 component matched
 component = 0 output = 0 component matched
 component = 0 output = 1 component not matched
* Unable to recall pattern B
```

meaning that the network was not trained to recall this pattern.

Source code

hopfield.h

{% highlight cpp linenos %}

class Neuron{
	private:
		int *weightsVec;
	public:
		Neuron(int *, int);
        friend class Hopfield;
};

class Hopfield{
	private:
		Neuron **neurons;
		int *output;
		int n;
		int threshold(int);
		int dotProduct(int *, int *, int);
	public:
		Hopfield(int **, int);
		void run(int  *);
		int getOutput(int);
};

#include "hopfield.cpp"

{% endhighlight %}

hopfield.cpp

{% highlight cpp linenos %}

Neuron::Neuron(int *w, int n){
	weightsVec = new int[n];
	for(int i = 0; i < n; i++)
		weightsVec[i] = w[i];
}

//n = wLen
//every neuron has its weight contribution to other neurons
Hopfield::Hopfield(int **w, int n){
	this -> n = n;
	output = new int[n];
	neurons = new Neuron*[n];

	for(int i=0;i<n;i++)
		neurons[i] = new Neuron(w[i],n);
}

//dot product pattern.weights (ej.  A.w1)
int Hopfield::dotProduct(int *pattern, int *weights, int wLen){
	int k = 0;
	for(int i = 0; i < wLen; i++)
		k += pattern[i] * weights[i];
	return k;
}

int Hopfield::threshold(int activation){
	if(activation >= 0) return 1; else return 0;
}

void Hopfield::run(int *pattern){
 int act;
 for(int i = 0; i < n; i++){
	 act = dotProduct(pattern, neurons[i]->weightsVec, n);
	 output[i] = threshold(act);
   }
}

//return output of neuron j
int Hopfield::getOutput(int j){
	return output[j];
}

{% endhighlight %}

main.cpp

{% highlight cpp linenos %}

#include <stdio.h>
#include <iostream>
#include <math.h>

#include "hopfield.h"

using namespace std;

int main(){
        // n is the number of neurons in the network
	const int N = 4;
	int recalled = 1;

	//patterns to recall
	int A[] = {1, 0, 1, 0}, B[] = {0, 1, 0, 1};

	//weight matrix
	int wm[N][N] = {
			{ 0, -3,  3, -3},
			{-3,  0, -3,  3},
			{ 3, -3,  0, -3},
			{-3,  3, -3,  0}
		       };

	int **w;
	w = new int*[N];
	for(int i = 0;i < N;i++) w[i] = wm[i];

	cout<<"*******************************"<<endl;
	cout<<"Hopfield Neural Network"<<endl;
	cout<<"able to recall binary patterns"<<endl;
	cout<<"*******************************"<<endl;

	Hopfield net(w, N);

	//run the network by presenting  pattern A.
	cout<<"Presenting pattern A"<<endl;
	net.run(A);

	for(int i = 0; i < N; i++){
		if(net.getOutput(i) == A[i]){
			cout<<" component = "<<A[i];
			cout<<" output = "<<net.getOutput(i);
			cout<<" component matched"<<endl;
		}
		else{
			cout<<" component = "<<A[i];
			cout<<" output = "<<net.getOutput(i);
			cout<<" component not matched"<<endl;
			recalled = 0;
		}
	}

	if(!recalled)
		cout<<"* Unable to recall pattern A "<<endl;
	else
		cout<<"* Pattern A recalled correctly"<<endl;

	recalled = 1;
	//run the network by presenting pattern B.
	cout<<"Presenting pattern B"<<endl;
	net.run(B);

	for(int i = 0; i < N; i++){
		if(net.getOutput(i) == B[i]){
			cout<<" component = "<<B[i];
			cout<<" output = "<<net.getOutput(i);
			cout<<" component matched"<<endl;
		}
		else{
			cout<<" component = "<<B[i];
			cout<<" output = "<<net.getOutput(i);
			cout<<" component not matched"<<endl;
			recalled = 0;
		}

	}

	if(!recalled)
		cout<<"* Unable to recall pattern B"<<endl;
	else
		cout<<"* Pattern B recalled correctly"<<endl;

	return 1;
}

{% endhighlight %}


