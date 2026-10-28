# AccountService

We discussed this Service earlier so we will discuss only the changes here.

# SaveAccount ()

This method invokes the corresponding repository's save method to save the account along with the updated score in the database. It also handles the logic of incrementing the scores for the account based on the ScoreInput variable. This score will be used to display medals for the user.

If the post is a reply to an existing question, then the post owner's account score is incremented by one. If the reply is voted as useful then the post owner's score is incremented by five and if it is voted as not useful the score is decremented by five. When decrementing we make sure that we don't go below zero. If the reply is marked as an answer then the post owner's account score is incremented by ten.

```
public void SaveAccount(Account account, BoardPost.ScoreInput value)
{
    if(account.AccountID > 0)
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
                if(account.Score >5)
                    account.Score -= 5;
                else
                    account.Score = 0;
                break;
            case BoardPost.ScoreInput.MarkedAsAnswer:
                account.Score = post.Account.Score + 10;
                break;
            default:
                break;
        }
    }
    __accountRepository.SaveAccount(account);
}
```