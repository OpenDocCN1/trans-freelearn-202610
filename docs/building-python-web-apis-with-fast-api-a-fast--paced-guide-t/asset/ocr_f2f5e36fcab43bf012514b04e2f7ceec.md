The result looks like so:

![](bbox=[0.115, 0.135, 0.85, 0.44])

Figure 8.8 – Successful POST request test run

Let’s write a test to verify the count of events stored in the database (in our case, 2). Add the following:

```
@pytest.mark.asyncio
async def test_get_events_count(default_client: httpx.AsyncClient) -> None:
    response = await default_client.get("/event/")

    events = response.json()

    assert response.status_code == 200
    assert len(events) == 2
```

In the preceding code block, we have stored the JSON response in the events variable, whose length is used for our test comparison. Let’s rerun the test file:

```
(venv)$ pytest tests/test_routes.py
```