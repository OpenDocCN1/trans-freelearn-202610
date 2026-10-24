```
 );

var existsEmail =
    repo.selectExistsEmail(user.getEmail());
if (existsEmail) throw new BadRequestException(
    "Email " + user.getEmail() + " taken"
);

byte[] salt = createSalt();
byte[] hashedPassword =
    createPasswordHash(password, salt);

user.setStoredSalt(salt);
user.setStoredHash(hashedPassword);

repo.save(user);

return convertToDto(user);
}
```

+   - **Updating and deleting a user:** The last two methods we need to add are `updateUser()` and `deleteUser()`. This is a different method we can implement to give us the capability to edit the details of the user or delete the user in the database.

Let's see the following code implementation:

```
public void updateUser(UUID id, UserDto userDto, String password)
    throws NoSuchAlgorithmException {
    var user = findOrThrow(id);
    var userParam = convertToEntity(userDto);

    user.setEmail(userParam.getEmail());
    user.setMobileNumber(userParam.getMobileNumber());

    if (!password.isBlank()) {
        byte[] salt = createSalt();
        byte[] hashedPassword =
```