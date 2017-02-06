# Lazy Dude's Database Normalization
0. Each table should map to only 1 model e.g. *products* table maps to your *Product* model
0. Each table row should map to only 1 instance of the model
0. Columns should not store collections, this includes duplicating columns with similar column names e.g. *|| Phone Nos ||* and *|| Phone No 1 | Phone No 2 | phone No 3 ||* are both bad.
0. Rows should avoid duplicate value data and should instead store references to other tables e.g. a *category* column on a *books* table should store foreign keys for entries on the *categories* table rather than a category name
0. Don’t store stuff you can work out easily from other object data

### Most Important Rule
You can break any of these rules, as long as it is done consciously and with good reason e.g. don’t make tables for the sake of making them. Don’t spend too much time design ERDs and lose focus of the product, especially when it is in the infancy stage - YAGNI.
