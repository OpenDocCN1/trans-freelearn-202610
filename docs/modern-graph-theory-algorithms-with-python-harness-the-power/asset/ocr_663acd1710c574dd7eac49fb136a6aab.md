Note that our heron network shows many different interaction patterns over the observation duration. This will impact our viral dynamics, as different animals interact at different time periods. Let’s consider an infectious disease that spreads rapidly (beta = 0.4) and resolves at a slower pace (gamma = 0.2). Bird flu epidemics tend to spread rapidly, and these parameters may represent a novel bird flu variant passing through a population. In a real epidemic of bird flu, longer latency to infection means we’d need to observe the population over a period of longer than 50 minutes (perhaps over the course of several days). However, let’s see how our proposed epidemic spreads through our heron population based on interactions by adding to Script 8.3:

```
```python
#run epidemic model
import ndlib.models.ModelConfig as mc
%matplotlib inline
from ndlib.viz.mpl.DiffusionTrend import DiffusionTrend
#from ndlib.viz.mpl.DiffusionPrevalence import DiffusionPrevalence
import ndlib.models.dynamic as dm

# model selection
model = dm.DynSIRModel(blue_heron)

# model configuration
config = mc.Configuration()
config.add_model_parameter('beta', 0.4) # infection rate
config.add_model_parameter('gamma', 0.2) # recovery rate
config.add_model_parameter("percentage_infected", 0.1)
model.set_initial_status(config)

# simulate snapshot based execution
iterations = model.execute_snapshots()
#iterations = model.execute_iterations()
trends = model.build_trends(iterations)

#visualize trends
viz = DiffusionTrend(model, trends)
viz.plot()
```
```