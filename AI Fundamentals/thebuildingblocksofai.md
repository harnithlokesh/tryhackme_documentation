1. AI vs Machine Learning
Artificial Intelligence (AI)

AI is the broad field of creating machines that can perform tasks that normally require human intelligence, such as reasoning, decision-making, understanding language, and recognising patterns.

Machine Learning (ML)

Machine Learning is a subfield of AI where systems learn patterns from data instead of being explicitly programmed with every rule.

Example:

Traditional programming: If email contains X → spam
Machine Learning: Give the model thousands of spam and legitimate emails → it learns the patterns itself.
Overfitting

Overfitting happens when a model becomes too familiar with its training data and performs poorly on new, unseen data.

In simple terms:

The model memorises instead of actually learning general patterns.

2. Types of Machine Learning

There are four major categories covered in the room.

Supervised Learning

The model learns from labelled data, meaning each training example has a known answer.

Example:

Give a model thousands of emails labelled spam or not spam.

Used for:

Spam detection
Classification
Price prediction
Unsupervised Learning

The model works with unlabelled data and tries to find patterns or structures by itself.

Example:

Give the model network traffic and ask it to find unusual behaviour.

Used for:

Clustering
Anomaly detection
Finding hidden patterns
Semi-Supervised Learning

Uses a small amount of labelled data together with a large amount of unlabelled data.

Example:

500 labelled images + 100,000 unlabelled images.

This is useful when labelling data manually is expensive or time-consuming.

Reinforcement Learning

An agent learns by taking actions and receiving rewards or penalties.

Basic cycle:

Take action → Receive reward/penalty → Learn → Try again

Example:

Game-playing AI
Robotics
Decision-making systems
3. Neural Networks

A neural network is a machine learning model inspired loosely by how neurons in the human brain work.

The basic structure is:

Input Layer → Hidden Layers → Output Layer
Input Layer

Receives the raw input data.

For example, if a model is processing a 4 × 4 image, the input could contain 16 pixel values.

Hidden Layers

Process the input and extract useful patterns and features.

As information passes through deeper layers, the network can learn increasingly complex patterns.

Output Layer

Produces the final prediction or classification.

Example:

Cat: 94%
Dog: 6%
Weights

Connections between neurons have weights.

A weight determines how much influence one neuron has on another.

The network changes these weights during training so that it becomes better at making predictions.

Synapses

The room uses the brain analogy and refers to the weighted connections between nodes as synapses.

Deep Learning

When neural networks contain multiple layers, they can be considered Deep Learning (DL) models.

4. Large Language Models (LLMs)

Large Language Models are AI models designed to understand and generate human language.

Examples include models behind tools such as ChatGPT and Gemini.

LLMs are trained using enormous amounts of text and learn patterns in language.

Parameters

LLMs contain billions of parameters.

Parameters are numerical values that the model adjusts during training to learn patterns and relationships in data.

You can think of them as the internal values that help determine how the model processes information and generates responses.

5. How LLMs Learn
Pre-Training

During pre-training, the model processes huge amounts of text and learns general patterns of language.

The model isn't simply memorising entire documents. It learns statistical relationships and patterns between tokens and concepts.

Backpropagation

Backpropagation is used to adjust the model's parameters based on how wrong its prediction was.

Simplified process:

Model makes prediction
        ↓
Compare prediction with correct answer
        ↓
Calculate error
        ↓
Backpropagate the error
        ↓
Adjust parameters
        ↓
Try again

This happens repeatedly during training.

6. Transformers

Modern LLMs are largely based on the Transformer architecture.

The Transformer architecture was introduced in Google's 2017 paper:

"Attention Is All You Need"

Transformers were a major breakthrough because they made it much easier for models to process language while considering the context of words.

7. Attention

Attention is a mechanism used by Transformer networks to determine which parts of a sequence are important when processing a particular word or token.

For example:

"The dog chased the ball because it was excited."

The model needs to understand what "it" refers to.

Attention allows the model to consider relevant surrounding words and their relationships.

In simple terms:

Attention helps the model figure out which words matter most to understanding the current context.

8. RLHF

RLHF = Reinforcement Learning from Human Feedback

It is used to improve a model's behaviour after its initial training.

Simplified:

AI generates responses
        ↓
Humans evaluate the responses
        ↓
Better responses are rewarded
        ↓
Model is fine-tuned
        ↓
Model learns to produce more useful responses

RLHF helps align model behaviour with what humans consider useful, safe, and appropriate.

9. Practical Challenges in the Room

The room doesn't only teach theory.

ARIA

ARIA is an AI agent used in the room's interactive missions.

You interact with ARIA and apply concepts from AI and Machine Learning to solve the challenges.

NEURON-1

NEURON-1 is the practical neural network challenge.

You interact with a neural network by:

Providing input
Working through the network
Understanding the hidden layer
Reading the output
Completing the classification task

This makes you apply the concepts instead of simply memorising definitions.

Quick Revision
Concept	Simple Meaning
AI	Broad field of making machines perform intelligent tasks
ML	AI that learns patterns from data
Overfitting	Model memorises training data and fails on new data
Supervised Learning	Learns from labelled data
Unsupervised Learning	Finds patterns in unlabelled data
Semi-Supervised Learning	Small labelled dataset + large unlabelled dataset
Reinforcement Learning	Learns through rewards and penalties
Neural Network	Model made of connected artificial neurons
Input Layer	Receives raw data
Hidden Layers	Process and extract features
Output Layer	Produces prediction
Weights	Values controlling the strength of connections
Synapses	Weighted connections between nodes
Deep Learning	Neural networks with multiple layers
LLM	Large model designed to understand/generate language
Parameters	Numerical values learned during training
Backpropagation	Adjusts parameters based on prediction error
Transformer	Neural network architecture used by modern LLMs
Attention	Determines which parts of the input are important
RLHF	Uses human feedback to improve model behaviour
The Big Picture

The concepts can be remembered as a progression:

Artificial Intelligence
        ↓
Machine Learning
        ↓
Neural Networks
        ↓
Deep Learning
        ↓
Transformers
        ↓
Large Language Models

And Machine Learning can be divided into:

Machine Learning
├── Supervised Learning
├── Unsupervised Learning
├── Semi-Supervised Learning
└── Reinforcement Learning
Cybersecurity Relevance

Understanding these fundamentals is useful because AI is increasingly being used in cybersecurity for:

Threat detection
Anomaly detection
Phishing detection
Malware classification
SOC automation
Fraud detection
Security monitoring
AI-powered security tools

At the same time, cybersecurity professionals also need to understand AI-specific threats, such as:

Prompt injection
Adversarial attacks
AI model manipulation
Data poisoning
LLM vulnerabilities
AI agent security
Deepfakes and voice fraud

The main takeaway from this room is:

AI is the broad concept, ML allows systems to learn from data, neural networks are one way of implementing ML, deep learning uses deeper neural networks, and modern LLMs rely heavily on Transformer architectures and attention.