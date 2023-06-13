query > request made to firestore to give something from the database

firestore returns us two types

- references and snapshots
- Of these, they can be either doc or collection versions

Firestore always return these objects. even if nothing exists at that query.

----

- use documentRef objects for CRUD
  1. .set()
  2. .get()
  3. .update()
  4. .delete()
- add documents to collections using the collectionRef object via .add() method
  - `collectionRef.add({//value:prop})`
- Get snapshot Object from the reference Object using .get() method
  - documentRef.get() or collectionRef.get()

- documentRef returns a documentSnapshot object
- CollectionRef returns a querySnapshot object.

https://stackoverflow.com/questions/49859954/firestore-difference-between-documentsnapshot-and-querydocumentsnapshot#:~:text=A%20QueryDocumentSnapshot%20contains%20data%20read,same%20API%20surface%20as%20DocumentSnapshot.