```
underTest.save(user);

// when
boolean expected = underTest.selectExistsEmail(email);

// then
assertThat(expected).isTrue();
}
```

We have added an example user entity into our database in the example unit test. The selectExistsEmail() method is expected to return true. This should retrieve the added user with the given email.

The next test is for the findByEmail() method; this is almost similar to the test we have created for the selectExistsEmail() method. The only thing we need to modify is the assertion, as we are expecting a return value of the User type.

Let’s have a look at the following example code:

```
@Test
void itShouldFindUserWhenEmailExists() {
    // give
    String email = "dennis@gmail.com";
    UserEntity user = new UserEntity(email, "21398732478");

    underTest.save(user);

    // when
    UserEntity expected = underTest.findByEmail(email);

    // then
    assertThat(expected).isEqualTo(user);
}
```

We have successfully written a test for our services and repository with JUnit, AssertJ, and the H2 database. In the next section, we will use the second implementation on writing unit tests using JUnit and AssertJ with Mockito.