<dmf:requiredfieldvalidator> is a WDK validator tag used for performing a mandatory check on the specified field (in the attribute controltovalidate). In our case the HTML form field txt_news_article_name is being checked by the validator. If the user does not provide a name for the news article, the WDK framework throws a message, the property string for which is specified in the nlsid key MSG_LBL_NEWS_ARTICLE_NAME_REQUIRED.

The string value for this property string is specified in the associated NLS property file NewNewsArticleNlsProp.property. We will discuss this in the subsequent steps.

```
<tr>
    <td>
        <dmf:label name="lbl_news_summary" nlsid="MSG_LBL_NEWS_SUMMARY" />
    </td>
    <td>
        <%--Text field where user fill the value of the News Article summary.--%>
        <dmf:text name="txt_news_summary" />
        <%--Documentum provider validator which checks that the News Article summary is a mandatory field.--%>
        <dmf:requiredfieldvalidator name="txt_news_summary_validator"
            controltovalidate="txt_news_summary"
            nlsid='MSG_LBL_NEWS_SUMMARY_REQUIRED' />
    </td>
</tr>
<tr>
    <td>
        <dmf:label name="lbl_display_order"
            nlsid="MSG_LBL_DISPLAY_ORDER" />
    </td>
    <td>
        <%--DataDropDownList showing display order of news articles.--%>
        <dmf:datadropdownlist name="txt_display_order">
            <dmf:dataoptionlist>
                <dmf:option datafield="DISPLAY_ORDER_ID"
                    labeldatafield="DISPLAY_ORDER_VALUE"/>
            </dmf:dataoptionlist>
        </dmf:datadropdownlist>
    </td>
</tr>
</table>
</dmf:form>
</body>
</html>
```

The <dmf:datadropdownlist> tag depicts a data dropdown control that translates into a dropdown in the form, showing the designated values in the list.