This equation now contains all computable values. Therefore, we can compute the sum of squared differences for the new set of data (the current one and the new batch) and, thus, the standard deviation.

The following code snippets implement these calculations. First, we need a helper method to broadcast the running means into the sizes compatible with the input sizes used in our calculation:

```
```python
def broad_cast(x, image_size, channel):
    y = torch.broadcast_to(x, (image_size**2, channel))
    z = y.reshape(image_size, image_size, 3)
    return torch.moveaxis(z, 2, 0)
```
```

The following code snippet utilizes this helper method and prepares a class to calculate running means and standard deviations from batches of inputs in a rolling/running manner:

```
```python
class RunningStats:
    def __init__(self):
        # initialize: self.n = 0, self.mean = 0, self.ssd = 0
    def push(self, x):
        dims = [0, 2, 3]
        count = 1
        for dim in dims:
            count *= x.shape[dim]
        image_size = x.shape[-1]
        channel = x.shape[1]
        if self.n == 0: # start
            # Step 1: compute new_mean, new_ssd, new_count
        else: # otherwise
            # Step 2: store old statistics and compute new ones
        # release results
        self.mean = new_mean
        self.ssd = new_ssd
        self.n = new_count
        self.std = torch.sqrt(new_ssd/(new_count-1))
```
```