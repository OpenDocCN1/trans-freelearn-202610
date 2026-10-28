

## 用户交互性

这种方法的下一层复杂度在于我们希望对云中的标签进行何种排序。我们是从小到大、从大到小，还是随机排序？我们将这交给一个配置值来决定。如果我们希望从小到大排序，可以在 Tags 集合上使用 `OrderBy()` 方法。如果我们希望从大到小排序，可以使用 `OrderByDescending()` 方法。如果我们想随机排序该怎么办？我们将向 Extensions 类添加一个新方法 `ShuffleList()`，它将扩展 `Tag` 类型的泛型列表，负责对集合中的项目进行随机打乱。稍后我们将讨论该方法。

```csharp
//Fisharoo/BusinessLogic/TagService.cs
public List<Tag> CalculateFontSize(List<Tag> Tags)
{
    decimal MinimumRange;
    decimal MaximumRange;
    decimal Delta;
    //get the smallest count in this list
    MinimumRange = (Tags.OrderBy(t => t.Count).Take(1).Select(t =>
                                      t.Count).FirstOrDefault()) * 100;
    //get the largest count in this list
    MaximumRange = (Tags.OrderByDescending(t =>
                                      t.Count).Take(1).Select(t => t.Count).
                                      FirstOrDefault()) * 100;
    //determine the difference between the minimum and the maximum
    Delta = MaximumRange - MinimumRange;
    if (Tags.Count > 1)
    {
        for (int i = 0; i < Tags.Count(); i++)
        {
            //set a working value
            Tags[i].InitialValue = Tags[i].Count*100;
            //calculate the minimum offset
            Tags[i].MinimumOffset = Tags[i].InitialValue -
                                    MinimumRange;
            //calculate the ranged value
            Tags[i].Ranged = Tags[i].MinimumOffset/Delta;
            //calculate the pre calculation
            Tags[i].PreCalculatedValue = Tags[i].Ranged*
                                         ((_configuration.TagCloudLargestFontSize -
                                         _configuration.AddToTagCloudFontSize) - 1);
            //calculate the final value
            Tags[i].FinalCalculatedValue = Tags[i].PreCalculatedValue
                                           + 1;
            //calculate the font size
            Tags[i].FontSize =
Convert.ToInt32(Tags[i].FinalCalculatedValue +
    _configuration.AddToTagCloudFontSize);
        }
    }
    //if a standard sort is not what you require, you can call
    //Tags.Sort();
    //  The Tags.Sort() method (in the Tag.cs partial class)
    //  can be
    //  modified to use different properties to sort by
    if (_sortOrder == CloudSortOrder.Ascending) //small to tall
    {
        Tags = Tags.OrderBy(t => t.FinalCalculatedValue).ToList();
    }
    else if (_sortOrder == CloudSortOrder.Descending) //tall to small
    {
        Tags = Tags.OrderByDescending(t =>
            t.FinalCalculatedValue).ToList();
    }
    else
    {
        Tags.ShuffleList(); //randomize!
    }
    return Tags;
}
```

## 扩展

我们将向 Extensions 类添加一个名为 `ShuffleList()` 的扩展方法，它负责打乱 Tags 列表中的项目。我们在 Experts-Exchange.com 上找到了这段代码：http://www.experts-exchange.com/Programming/Languages/C_Sharp/Q_22571864.html

```csharp
//Fisharoo/Common/Extensions.cs
public static List<Tag> ShuffleList(this List<Tag> listToShuffle)
{
    Random randomClass = new Random();
    for (int k = listToShuffle.Count-1; k > 1; --k)
    {
        int randIndx = randomClass.Next(k); //
        Tag temp = listToShuffle[k];
        listToShuffle[k] = listToShuffle[randIndx]; // move random
                                                   // num to end of list.
        listToShuffle[randIndx] = temp;
    }
    return listToShuffle;
}
```

## AccountService

我们之前已经讨论过这个服务，因此这里只讨论所做的更改。

### SaveAccount()

该方法调用相应仓库的保存方法，将账户及其更新后的分数保存到数据库中。它还处理根据 ScoreInput 变量增加账户分数的逻辑。该分数将用于向用户显示勋章。

如果帖子是对已有问题的回复，则帖子所有者的账户分数增加 1。如果该回复被投票为有用，则帖子所有者的分数增加 5；如果被投票为无用，则分数减少 5。在减少时，我们确保分数不会低于零。如果该回复被标记为答案，则帖子所有者的账户分数增加 10。

```csharp
public void SaveAccount(Account account, BoardPost.ScoreInput value)
{
    if (account.AccountID > 0)
    {
        switch (value)
        {
            case BoardPost.ScoreInput.ReplyPost:
                account.Score += 1;
                break;
            case BoardPost.ScoreInput.VoteUseful:
                account.Score += 5;
                break;
            case BoardPost.ScoreInput.VoteNotUseful:
                if (account.Score > 5)
                    account.Score -= 5;
                else
                    account.Score = 0;
                break;
            case BoardPost.ScoreInput.MarkedAsAnswer:
                account.Score += 10;
                break;
            default:
                break;
        }
    }
    _accountRepository.SaveAccount(account);
}
```

## WebContext

这里我们将讨论对 WebContext 类所做的一些更改。

### SelectedRatings

SelectedRatings 属性处理存储在 Session 中的 `Dictionary<int, int>` 集合。该集合负责处理附加到对象上各个选项的评分。第一个 int 是 RatingOptionRecordID，第二个 int 是应用于该 SystemObjectRatingOptionID 的评分（分数）。该集合在用户保存其评分选项之前构建。我们的意思是，每次用户向 SelectedRatings 属性添加字典项时，它都会将该项目添加到存储在 Session 中的集合中，而不是完全重置该值。当我们开始构建 Ratings 控件时，就会看到这一点是如何体现的。

如果你想知道这个集合是如何处理的——如果它持续增长——你应该知道还有另一个方法（接下来会介绍）负责清除这个集合。

```csharp
//Fisharoo/Components/WebContext.cs
public Dictionary<int, int> SelectedRatings
{
    get
    {
        Dictionary<int, int> result = new Dictionary<int, int>();
        if (GetFromSession("SelectedRatings") != null)
        {
            result = GetFromSession("SelectedRatings") as Dictionary<int, int>;
        }
        return result;
    }
    set
    {
        //make sure that we add to the existing rating store rather
        //than creating a new one
        Dictionary<int, int> result = new Dictionary<int, int>();
        if (GetFromSession("SelectedRatings") != null)
        {
            result = GetFromSession("SelectedRatings") as Dictionary<int, int>;
            foreach (KeyValuePair<int, int> pair in value)
            {
                if (!result.ContainsKey(pair.Key))
                    result.Add(pair.Key, pair.Value);
            }
            SetInSession("SelectedRatings", result);
        }
        else
            SetInSession("SelectedRatings", value);
    }
}
```

### ClearSelectedRatings

该方法只是清除存储在 SelectedRatings 背后会话中的 `Dictionary<int, int>` 集合。

```csharp
//Fisharoo/Components/WebContext.cs
public void ClearSelectedRatings()
{
    SetInSession("SelectedRatings", null);
}
```

## 配置

Configuration 类负责读取配置文件中的值。在最后一轮编码功能中，我们添加了以下属性：

- TagCloudLargestFontSize：该值设置标签云中显示的最大字号。
- TagCloudSmallestFontSize：该值设置标签云中显示的最小字号。
- CloudSortOrder：该值设置标签云中标签的排序顺序。
- NumberOfTagsInCloud：该值设置要在标签云中显示的标签数量。

## 实现表示层

本章主要介绍如何创建必要的用户控件——Comments、Ratings、Tags、Votes 和 MarkAnswer。除了 Tags 控件之外，我们还有一个 Tags.aspx 页面，负责显示指定 Tag 的所有关联项。

### 评论页面

让我们先来看看评论页面。

