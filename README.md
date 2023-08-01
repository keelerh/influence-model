influence-model
========

`influence_model` is a Python implementation of the influence model, a generative model that describes the interactions between networked Markov chains.

**Why _influence_model_?** It provides an efficient and well-documented implementation of Asavathiratham's original influence model and supports defining new influence models as well as generating observations by applying the model's evolution equations.

**Installation:** Install _influence-model_ by: 

```
pip install influence-model
```

**Example Usage:**
We define two sites (nodes) in the network, a leader and a follower. The follower always copies the behavior of the leader. Both sites have two possible statuses, `0` or `1`, represented by indicator vectors. We also define a network matrix $D$ and a state-transition matrix $A$ to instantiate the influence model.

```python
import numpy as np

from influence_model.influence_model import InfluenceModel
from influence_model.site import Site

leader = Site('leader', np.array([[1], [0]]))
follower = Site('follower', np.array([[0], [1]]))
D = np.array([
    [1, 0],
    [1, 0],
])
A = np.array([
    [.5, .5, 1., 0.],
    [.5, .5, 0., 1.],
    [.5, .5, .5, .5],
    [.5, .5, .5, .5],
])
model = InfluenceModel([leader, follower], D, A)
initial_state = model.get_state_vector()
print(initial_state)
```

The initial state of the network is a vector stack of the initial statuses of the two sites.

```python
[[1]
 [0]
 [0]
 [1]]
```

Next, we apply the evolution equations of the influence model to progress to the next network state.

```python
next(model)
next_state = model.get_state_vector()
print(next_state)
```

We see that the follower has adopted the previous status of the leader.

```python
[[0]
 [1]
 [1]
 [0]]
```

This following behavior continues through all subsequent iterations.

**Acknowledgements:** If you find this library helpful to your work, please cite the following paper:

```
@article{Erhardt_Hidden_Messages_Mapping_2023,
    author = {Erhardt, Keeley and Pentland, Alex},
    doi = {10.0000/00000},
    journal = {Computational and Mathematical Organization Theory},
    month = sep,
    number = {3},
    pages = {1--10},
    title = {{Hidden Messages: Mapping Nations’ Media Campaigns}},
    volume = {29},
    year = {2023}
}
```
