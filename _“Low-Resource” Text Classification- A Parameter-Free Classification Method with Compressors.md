# Summary

## Idea

Basically use gzip to compress the text, which is then used as a representation of the text.

The idea is that similar texts will have similar length of concatenation.

C(x) = compressed length

- x1 = Japan’s Seiko Epson Corp. has developed a 12-gram flying microrobot.
- x2 = The latest tiny flying robot has been unveiled in Japan.
- x3 = Michael Phelps won the gold medal in the 400 individual medley.

Since x2 is simlar to x1, and much different to x3, C(x1+x2) - C(x1) < C(x1+x3) - c(x1)

We can then use a distance-based classifier (like KNN) to classify texts based on the computed distance.

## Performance

- This is much faster and requires less resource than using DNN approaches
- Performs really well when there's not much labeled data: Outperformed DNN approaches in few-shot learning. As we increase the number of shots, both gzip and DNN performances become similar.
- gzip turned out to be the most well rounded in terms of performance and accuracy

```python
import gzip
import numpy as np

for(x1, _) in test_set:
    # Compress X in test set
    Cx1 = len(gzip.compress(x1.encode()))

    distance_from_x1 = []

    for(x2, _) in training_set:
        # Compress X in training set
        Cx2 = len(gzip.compress(x2.encode()))

        # Compute distance between the example in the test set and all the examples in
        # the training set
        x1x2 = "".join([x1,x2])
        Cx1x2 = len(gzip.compress(x1x2.encode()))
        ncd = (Cx1x2 - min(Cx1,Cx2)) / max(Cx1, Cx2)

        distance_from_x1.append(ncd)

# Get top p
sorted_idx = np.argsort(np.array(distance_from_x1))
top_k_class = training_set[sorted_idx[:k], 1]
predict_class = max(set(top_k_class), key=top_k_class.count)
````
