
**delete a list of items :**
- deletes everything in the array.
```ts
import ContentModel from "../models/content.model";

const titlesToDelete = ["How to Node", "DSA Sheet", "Random Notes"];

const deleted = await ContentModel.deleteMany({
  title: { $in: titlesToDelete },
});

console.log(`${deleted.deletedCount} documents deleted.`);

```

