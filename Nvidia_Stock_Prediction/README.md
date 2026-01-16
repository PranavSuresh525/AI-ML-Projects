## The First idea I try to implement-LSTM
This project was a challenge I took, to test myself. I started researching on the topic and came to know the shear complexity of time series forcasting used in stock markets
A name that kept popping up was an LSTM model. Its advantage over an RNN model( A model that maintains a hidden state that feeds from the output of a neural network and uses it to influence future decisions)
RNNs tend to use outdated data to influence the decisions which can hinder the results we want in a stock market prediction. Vanishing and exploding gradients also have a higher likely hood of manifesting
LSTMs have an additional memory cell which has input gate, forget gate amd output gate. Forget gate is placed before input gate, thus the non relevant information is discarded and allows better up-to date predictions

## Second idea I tried to implement- LSTM-Tansformer Hybrid
I was searching ways to improve my model as I was not satisfied with the output, upon googling and asking chatgpt for a while, I stumbled upon the idea of LSTM-Transformer hybrids
The thought process was that i needed a way to capture the large scale changes happening, this is done best by transformers. Implementing this was very hard as it had a complicated setup with a the core of the transformer having a "context" block which is a softmax with the output matrix of the lstm model thus capturring past trends all at once.However, despite training for 150 epochs, I was a left with a large loss rate and a model which was able to capture the trends but was consistently predicting less than the required stock prices. It took a lot of time to train (even with a a gpu) and produced worse results than a simple AR model Thus I needed a shift in model again.

## The Third and Last idea- LSTM-CNN-AR Hybrid Model
While looking at the comparision graphs of all the models (AR, MA, VAR, LSTM-Transformer hybrid, LSTM) AR graph performed the best despite being the simplest of them all. Thus the idea of integrating the an ar layer in my model came. I researched online a bit when I found that CNN-LSTM models are already in use for finance predictions. Thus I added an AR layer to this network and the LSTM_AR_CNN_Hybrid model was born. 

The model starts parallely with an AR and CNN layer. The AR looks back (this case for 10 days) to predict it for a week. Due to this the AR model has a [32, 10] input tensor. AR model is used here as in case the LSTM-CNN part of the model makes wild prediction it must be tied to the AR model, which assumes a linear relationships thus keeping the extreme values in check. 
Thus our CNN-LSTM part has to now only predict the trends on top of the AR models linearity. Parallely, the CNN block which has 2 convolution layer extracts the infor from the past trends by using the 'kernels' in the form of a [32, 1, 30] tensor ( 32 batches, 1 feature, 30 days).
This is now fed into the LSTM whcih processes the data linearly and forgets the irrelevent ones (recall the forget gates). The output from this layer is a [32, 128] tensor, which is fed finally into a residual head which uses the input from LSTM to predict the 'gap' from the linear trend.
Thus this is done and a matrix of [32, 7] is given out which represents the batrch of 32 fed into the model giving a 7 day prediction.
The Model Architecture should look something like this-

<img width="1079" height="899" alt="Image" src="https://github.com/user-attachments/assets/4d72db0f-3f65-4753-b362-782c8cabbdda" />
                                                      
The training loop is an ordinary one with Huber loss function ( the most used one for time series one), it has ADAM optimizer with a weighted decay to smooten the learning curve. I finally also implemented a scheduler to modify the learning rate on the fly. I set the patience to 20 to avoid overfitting of the model. Thus the model, even though having to train to 200 epochs usually stops by th 70-80th one due to this.

The results for this is evaluated using all the averages ( RME, MAE, MAPE). A graph also is shown plotting all the models side by side and comparing the best one.
