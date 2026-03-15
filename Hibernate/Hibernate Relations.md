mappedBy → defines inverse side / non-owning side

@JoinColumn → defines owning side

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