You will need your Databricks token to be able to issue the request to the endpoint. As mentioned in previous chapters, you can get your token in the **User Settings** page in Azure Databricks.

Before being able to make requests to our endpoint, remember to enable the serving on the model page in the Azure Databricks workspace. You can see a reminder of how to do this in the following screenshot:

![](bbox=[0.13, 0.25, 0.85, 0.42])

Figure 8.15 – Enabling model serving

+   1. Finally, we can call our model. The following example function will issue a Pandas data frame and will return the inference for each row in the DataFrame:

```
import os
import requests
import pandas as pd
def score_model(dataset: pd.DataFrame):
    url = 'https://YOUR_DATABRICKS_URL/model/wine_quality/Production/invocations'
    headers = {'Authorization': f'Bearer {your_databricks_token}'}
    data_json = dataset.to_dict(orient='split')
    response = requests.request(method='POST',
    headers=headers, url=url, json=data_json)
    if response.status_code != 200:
        raise Exception(f'Request failed with status {response.status_code}, {response.text}')
    return response.json()
```