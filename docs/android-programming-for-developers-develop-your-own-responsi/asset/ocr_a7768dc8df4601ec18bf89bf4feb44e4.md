The R.id.btnInsert and R.id.btnDelete cases simply pass the contents of the appropriate EditText widgets to the insert and delete methods of the DataManager class respectively.

Add the onClick method to the MainActivity class:

```
@Override
public void onClick(View v){
	switch (v.getId()){
	case R.id.btnInsert:
	dm.insert(editName.getText().toString(),
	editAge.getText().toString());
	break;

	case R.id.btnSelect:
	showData(dm.selectAll());
	break;

	case R.id.btnSearch:
	showData(dm.searchName(editSearch.getText().toString()));
	break;

	case R.id.btnDelete:
	dm.delete(editDelete.getText().toString());
	break;
	}
}
```

Let's take a look at the app in action.