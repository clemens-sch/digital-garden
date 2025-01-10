#Software-Engineering 

---
## # Features ?

ML-Algorithms prefer `numerical inputs` instead of `text` (strings).
We use Feature-Engineering to convert these numerical inputs in human-readable text.

---
## # One-Hot-Encoding

Is used when working with categorical variables:

| Car   | Color |
| ----- | ----- |
| BWM   | Blue  |
| Audi  | Red   |
| Tesla | White |
When you want to give a model a feature, assign numbers to these categorical variables:

| Car   | Blue | Red | White |
| ----- | ---- | --- | ----- |
| BMW   | 1    | 0   | 0     |
| Audi  | 0    | 1   | 0     |
| Tesla | 0    | 0   | 1     |
