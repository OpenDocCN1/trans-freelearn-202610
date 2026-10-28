# Default.aspx

With the entire infrastructure completed, we can now move on to the more interesting part—displaying the data. We will start by looking at the homepage. This page will be responsible for displaying all of the categories and all of the forums for each category as shown in the first screenshot of this chapter.

Other than the normal plumbing that is required by the **Model View Presenter (MVP)** pattern, this page is primarily made up of two repeaters—the first repeater displaying all the categories in the list of categories and the inner one displaying all the forums for each category.

The list of categories is loaded in the presenter for this page. The relevant method is

```
```csharp
//Fisharoo/Web/Forums/Presenters/DefaultPresenter.cs
public void Init(Default View)
{
    _view = View;
    List<BoardCategory> list = _boardService.GetCategoriesWithForums();
    _view.LoadCategories(Translator.BoardCategoryToPEBoardCategory(list));
}
```
```

Once the page's code behind passes control from itself to the presenter by way of calling the presenter's **init** method, the presenter then passes a list of categories and forums (from the BoardService) back to the code behind via the LoadCategories method. Note also the usage of PEBoardCategory presentation entity. We have discussed presentation entities in Chapter 5, so we will skip the explanation here.

The code behind then loads the data source of the first repeater and binds it.

```
```csharp
//Fisharoo/Web/Forums/Default.aspx.cs
public partial class Default : System.Web.UI.Page, IDefault
{
    private DefaultPresenter _presenter;
    protected void Page_Load(object sender, EventArgs e)
    {
        _presenter = new DefaultPresenter();
        _presenter.Init(this);
    }

    public void LoadCategories(List<PEBoardCategory> Categories)
    {
        repCategories.DataSource = Categories;
        repCategories.DataBind();
    }
}
```
```