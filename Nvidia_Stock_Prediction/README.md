## The First idea I try to implement-LSTM
This project was a challenge I took, to test myself. I started researching on the topic and came to know the shear complexity of time series forcasting used in stock markets
A name that kept popping up was an LSTM model. Its advantage over an RNN model( A model that maintains a hidden state that feeds from the output of a neural network and uses it to influence future decisions)
RNNs tend to use outdated data to influence the decisions which can hinder the results we want in a stock market prediction. Vanishing and exploding gradients also have a higher likely hood of manifesting
LSTMs have an additional memory cell which has input gat, forget gate amd output gate. Forget gate is placed before input gate, thus the non relevant information is discarded and allows better up-to date predictions

## Second idea I tried to implement- LSTM-Tansformer Hybrid
I was searching ways to improve my model as I was not satisfied with the output, upon googling and asking chatgpt for a while, I stumbled upon the idea of LSTM-Transformer hybrids
This is perfect as LSTM's only look at data linearly, which is good only for short term predictions but doesnt allow the Model to get a larger picture of the trends. Attaching a transformer to the LSTm architecture,
our model is able to get an idea of the trends going on and is able to predict with much more accuracy (clearly seen later when i compare both the models). By using the best of both worlds, i.e 
global trends using a transformer and local using an LSTM, one can make a very accurate prediction. However I had to learn Transformer architecture for this. Thus a few more hours of research went by. The result of the research is the model in the code with this file. 
Lets start with the architecture of the model; 
