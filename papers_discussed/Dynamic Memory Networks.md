# Title
[Dynamic Memory Network for Natural Language Processing](https://arxiv.org/pdf/1506.07285.pdf)

# Describe the method

## Architecture
architecture consists of
*Input Module
*Question Module
*Episodic Memory Module
*Answer Module

### Input Module
The input module vectorizes the input. The Input may be a sentence, review, article or a story.

### Question Module
It vectorizes the input question and feeds it to the episodic memory module.

### Episodic Memory Module
The episodic memory module chooses which parts of the inputs to focus on through the attention mechanism. It then produces a ”memory” vector representation taking into account the question as well as the previous memory. Each  iteration provides the module with newly relevant information about the input. In other words,the module has the ability to retrieve new information, in the form of input representations, which were thought to be irrelevant in previous iterations.

#### Multiple Episodes
Multiple iteration in episodes ensures that it attends to different inputs during each pass.It also allows for a type of transitive inference, since the first pass may uncover the need to retrieve additional facts.

#### Attention Mechanism
The scoring function takes as input the feature set and produces a scalar score. The scoring function is a simple two-layer feed forward neural network.

#### Memory Update Mechanism
Employs a modified GRU over the sequence of inputs weighted by gates. The episode vector that is given to the answer module is the final state of the GRU.

#### Criteria for Stopping
It appends a special end-of-passes representation to the input, and stop the iterative attention process if this representation is chosen by the gate function.

### Answer Module
The answer module generates an answer from the final memory vector of the memory module.

## Dataset
Facebook’s bAbI dataset

# Any further details
## NLP Application
The DMN has high QnA capabilities.

