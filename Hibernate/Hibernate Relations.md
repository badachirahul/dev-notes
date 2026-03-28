**mappedBy** → 
* `mappedBy is used to make relationship bidirectional`
* **Role of mappedBy:** `👉 Defines inverse side (non-owning side)`

**@JoinColumn** → 
* `Defines owning side`

cascade → propagates operations between entities

Bidirectional → both entities reference each other.

| Mapping                       | What happens          |
| ----------------------------- | --------------------- |
| `Passport → Student` only     | Unidirectional        |
| Both sides (`mappedBy`)       | Bidirectional         |
| `student.getPassport()` works | Because of `mappedBy` |


### Golden rule of JPA bidirectional relationships
* Always set both sides:
    ```
    student.setPassport(passport);
    passport.setStudent(student);
    ```
<br>

* Many-to-Many + join table allows:
    - Reuse
    - No duplication
    - Better filtering
    - Better searching
    - Clean database design


## Cascade:
* `Whatever DB operation is performed on the Parent, the same operation is automatically performed one the Child as well.`
* CascadeType.ALL means — whatever you do to Post, do the same to Tag:
    - Delete post → also delete the tag ❌ (not what you want!)
    - Save post → also save the tag ✅
    - Update post → also update the tag ✅

* Available cascade types:
    ```
    PERSIST — save parent → also save child
    MERGE — update parent → also update child
    REMOVE — delete parent → also delete child ⚠️
    ALL — all of the above
    DETACH — detach parent → also detach child
    ```