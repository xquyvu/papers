# Summary

## Link

<https://blog.picnic.nl/generating-your-shopping-list-with-ai-recommendations-at-picnic-300e716241db>

## Idea

Next item predictions. For each customer basket, create multiple subsequences, then train a NN to predict the next item for each subsequence. Then fill the basket with items with the highest average predicted scores across all subsequences.

## Background

- Data sparsity: More than 30k articles for grocery. Average customer only bought ~200 / 30k products
- Similar customers prefer slightly different versions of the same items.

## Approach

- Treat this as a sequential recommendation problem (predicting the next item). Open source libary `RecBole`. Github: <https://github.com/RUCAIBox/RecBole>
- For each customer basket, create multiple subsequences, then train a NN to predict the next item for each subsequence. Then fill the basket with items with the highest average predicted scores across all subsequences.

## Results

- Outperformed all current models by large margins (often double digits).
- Did really well on the repeat side, because
  - They had customer rebuy patterns. More than half of the shopping done is repeat behaviour
  - Repeat performance is often 10x better than explore performance.
  - -> A simple repeat model may work really well.
- Admit that they're still trying to work on the explore side.

## Potential directions

- Model at category level instead -> explore
- Maybe predicting only the next item is too strict. What about predicting bought items for the next few orders? (It's actually an easier task.)
