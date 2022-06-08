# Why PyTorch Lightning?
 - As Keras provides a high-level interface to TensorFlow, PyTorch Lightning is a library for providing a high-level interface to Pytorch.
 - This library separates the model code from the engineering code to keep your code clean and makes it easy to use a variety of training techniques, including multi gpus, 16-bit training and more in just a few lines of code.
 - Moreover, it provides convenience such that model.eval() and torch.no_grad() are called automatically during testing.


# GPU of PyTorch Lightning?
- When no GPU is available, it uses the CPU, and if there is more than one GPU, it is used to train the model.  
- If there are more than 2 GPUs, distributed learning is automatically performed using multiple GPUs.  
- The default setting is the distributed data parallel method (ddp_spawn) that spawns processes.


Before moving on to writing the code, let's first install PyTorch Lightning.

```shell
$ pip install pytorch-lightning torchmetrics tensorboard
```

```python
from pytorch_lightning import LightningModule, Trainer
from torchmetrics import Accuracy
```

# Structure of The PyTorch Lightning

```python
# model
model = CIFAR10Classifier()

# data
dm = CIFAR10DataModule()

trainer = Trainer(gpus=1)
trainer.fit(model, dm)
```



# LightningModule and Traniner
## Implementing the Lightning Module  
 - In PyTorch Lightning, you need to implement the Lightning Module, which is a superclass of nn.Module of PyTorch, so that the model and trainer can interact.
 - In order to define the Ligthning module, you need to inherit the **LightningModule class**.
 - And implement the model, training, validation, test loop, and optimizer. 
 - There are many methods that can be overridden, but among those methods, **training_step** and **configure_optimizer**s are methods that must be implemented.

```python
class LitMNIST(LightningModule):
    def __init__(self, hidden_size=64):
        super().__init__()
        self.model = nn.Sequential(
            nn.Flatten(),
            nn.Linear(28 * 28, hidden_size),
            nn.ReLU(),
            nn.Dropout(),
            nn.Linear(hidden_size, hidden_size),
            nn.ReLU(),
            nn.Dropout(),
            nn.Linear(hidden_size, 10)
        )
        self.accuracy = Accuracy()  

    def forward(self, x):
        pass
        
    def training_step(self, batch, batch_idx):
        pass

    def validation_step(self, batch, batch_idx):
        pass

    def test_step(self, batch, batch_idx):
        pass

    def configure_optimizers(self):
        pass
```


Now, let's redefine these undefined methods. 

### forward()
Although it is not a method that must be defined like nn.Module,
it makes it usable like self(input), so implementing it is convenient when implementing other methods.
```python
def forward(self, x):
	x = self.model(x)
    return F.log_softmax(x, dim=1)
```

### training_step(self, batch, batch_idx)
In the ```training_step()```, the batch provided by the training data loader and the index of the batch are given as arguments, and the training loss is calculated and returned. PyTorch Lightning conveniently changes the batch's tensor to a CPU, GPU, TPU or IPU tensor without adding a extra code to cast the type of the batch.

In this example, we find and pass the negative log-likelihood (NLL) loss between the model output and the correct answer label.

```python    
def training_step(self, batch, batch_idx):
	x, y = batch
    logits = self(x)
    loss = F.nll_loss(logits, y)
    return loss
```

### validation_step(self, batch, batch_idx)
```validation_step()``` is used to check the performance of the model in the middle of training. 
Similar to ```training_step()```, you can record the statistic you want to check with the batch provided by the validation data loader.
When storing one value, it can be saved as self.log(<variable name="">, <value>).  If you want to store multiple variables, you can save a dictionary with variable name and value pairs as self.log_dict() as in the example below.  
For each step, the average of the values ​​stored in the variable becomes the final value of that variable. 
Calling self.log() will surface up scalars for you in TensorBoard
Unless you specifically change the setting, 'val_loss' among the variables is used as the criterion for finding the best model.

In the example below, we obtain and save the model's negative log-likelihood loss and accuracy. 

Instead of ```accuracy()``` below, you can use the alternative in ```pytorch_lightning.metrics.functional```.

```python
def validation_step(self, batch, batch_idx):
	x, y = batch
    logits = self(x)
    loss = F.nll_loss(logits, y)

    preds = torch.argmax(logits, dim=1)
    self.accuracy(preds, y)
	
    self.log("val_loss", loss, prog_bar=True)
    self.log("val_acc", self.accuracy, prog_bar=True)

    # Or
    metrics = {"val_loss": loss, "val_acc": self.accuracy}
    self.log_dict(metrics)
    return loss
```
  
### test_step(self, batch, batch_idx)
Here we just reuse the validation_step for testing
  
```python
def test_step(self, batch, batch_idx):
    return self.validation_step(batch, batch_idx)
```

### configure_optimizers()
  
```configure_optimizers()``` implements an optimizer and learning rate (lr) scheduler. 
If you need to use multiple optimizers to train multiple models, such as GANs (they have both generator and discriminator), you can return them as a list. In this case, the optimizer's index is additionally received from ```training_step()```, and multiple models are trained alternately. 
  
```python
def configure_optimizers(self):
	optimizer = torch.optim.Adam(self.parameters(), lr=self.learning_rate)
    return optimizer
```
  
or with multiple optimizers and lr schedulers
  
```python
def configure_optimizers(self):
    g_opt = torch.optim.Adam(self.G.parameters(), lr=1e-5)
    d_opt = torch.optim.Adam(self.D.parameters(), lr=1e-5)
    return [g_opt, d_opt], [ReduceLROnPlateau(g_opt, ...), OneCycleLR(d_opt, ...)]
```
  
[HOW TO ORGANIZE PYTORCH INTO LIGHTNING](https://pytorch-lightning.readthedocs.io/en/stable/common/optimizers.html)

# Training and Test
## DataModule

 
Inside the ```LightningModule```, define the data related methods.

```python

class LitMNIST(LightningModule):
    def __init__(self):
        super().__init__()
        ...

        ####################
        # DATA RELATED HOOKS
        ####################

        def prepare_data(self):
            # download
            MNIST(self.data_dir, train=True, download=True)
            MNIST(self.data_dir, train=False, download=True)

        def setup(self, stage=None):
            # Assign train/val datasets for use in dataloaders
            if stage == "fit" or stage is None:
                mnist_full = MNIST(self.data_dir, train=True, transform=self.transform)
                self.mnist_train, self.mnist_val = random_split(mnist_full, [55000, 5000])

            # Assign test dataset for use in dataloader(s)
            if stage == "test" or stage is None:
                self.mnist_test = MNIST(self.data_dir, train=False, transform=self.transform)

        def train_dataloader(self):
            return DataLoader(self.mnist_train, batch_size=BATCH_SIZE)

        def val_dataloader(self):
            return DataLoader(self.mnist_val, batch_size=BATCH_SIZE)

        def test_dataloader(self):
            return DataLoader(self.mnist_test, batch_size=BATCH_SIZE)
```
Or can use **DataModule** of PyTorch Lightning: [LightningDataModule](https://pytorch-lightning.readthedocs.io/en/stable/extensions/datamodules.html)


## Trainer
By using the Trainer you automatically get: 
1. Tensorboard logging 
2. Model checkpointing 
3. Training and validation loop 
4. early-stopping

After defining the Trainer and the lightning module, you can train the model with Trainer's ```fit()``` function. Pass the model, training data loader and validation data loader as parameters of fit.
```python
model = LitMNIST()
trainer = Trainer(
    gpus=AVAIL_GPUS,
    max_epochs=3,
    progress_bar_refresh_rate=20,
)
trainer.fit(model)
```
Or if using the dataloader outside of the Lightning Module,
```python
trainer.fit(model, train_dataloader, val_dataloader, test_dataloader)
```

# Reference
[INTRODUCTION TO PYTORCH LIGHTNING](https://pytorch-lightning.readthedocs.io/en/latest/notebooks/lightning_examples/mnist-hello-world.html)  
[HOW TO ORGANIZE PYTORCH INTO LIGHTNING](https://pytorch-lightning.readthedocs.io/en/latest/starter/converting.html)  
[LIGHTNINGDATAMODULE](https://pytorch-lightning.readthedocs.io/en/stable/extensions/datamodules.html)  