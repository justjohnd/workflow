**Add new field to all documents in a collection**:
```
db.your_collection.updateMany(
  {},
  { $set: {"new_field": 1} }
)
```
