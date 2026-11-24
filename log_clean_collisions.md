# Data Cleaning Log — London Road Collisions

why chose this dataset

|  | Action | Reason | Rows affected |
|---|---|---|---|
| 1 | Kept collisions within Greater London plus 400m | Select collisions by their recorded coordinates | 401454 |
| 2 | Kept casualty rows with `casualty_class = 3` and `age_of_casualty` from 4 to 11 inclusive | Select pedestrian casualties aged 4–11 | 641654 |
| 3 | | | |
