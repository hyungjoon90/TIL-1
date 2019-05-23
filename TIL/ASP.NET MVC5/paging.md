# ASP.NET MVC Paging

페이징을 오픈소스를 이용해도 되지만 원리를 이해하면 간단하기 때문에 직접 만들어서 사용하는 것을 추천한다.

```csharp
public class PagingModel<T>
{
    public IEnumerable<T> Source { get; set; }

    public int Page { get; set; }

    public int PageSize { get; set; }

    public Decimal TotalCount { get; set; }

    public int CategoryFlag { get; set; }

    public int TotalPage => (int)Math.Ceiling(TotalCount / PageSize);

    public string Subject { get; set; }

    public string PaginationString()
    {
        var tensPage = (Math.Truncate((Page - 1) / 10.0) * 10);
        var totalPage = TotalPage;
        StringBuilder sb = new StringBuilder();
        bool isPreviousEnd = true;
        bool isNextEnd = true;

        if (tensPage == 0)
        {
            isPreviousEnd = false;
        }

        sb.Append("<ul class='pagination'>");

        if (isPreviousEnd)
        {
            sb.Append("<li class='paginate_button page-item before'>");
            sb.AppendFormat("<a class='{1}' data-page-value='{2}' href='javascript:'>{0}</a>", "before", "page-link2", tensPage - 9);
            sb.Append("</li>");
        }

        if (Page > 1)
        {
            sb.Append("<li class='paginate_button page-item previous2'>");
            sb.AppendFormat("<a class='{1}' data-page-value='{2}' href='javascript:'>{0}</a>", "previous", "page-link2 ", Page - 1);
            sb.Append("</li>");
        }

        for (int i = 1; i < 11; i++)
        {
            var pageValue = tensPage + i;
            sb.AppendFormat("<li class='paginate_button page-item {0}'>", pageValue == Page ? "active2" : "");
            sb.AppendFormat("<a class='{1}' data-page-value='{2}' href='javascript:'>{0}</a>", pageValue, "page-link2", pageValue);
            sb.Append("</li>");
            if (pageValue >= totalPage)
            {
                isNextEnd = false;
                break;
            }
        }

        if (Page < TotalPage)
        {
            sb.Append("<li class='paginate_button page-item next2'>");
            sb.AppendFormat("<a class='{1}' data-page-value='{2}' href='javascript:'>{0}</a>", "next", "page-link2", Page + 1);
            sb.Append("</li>");
        }

        if (isNextEnd)
        {
            sb.Append("<li class='paginate_button page-item after'>");
            sb.AppendFormat("<a class='{1}' data-page-value='{2}' href='javascript:'>{0}</a>", "after", "page-link2 ", (tensPage + 10 + 1) < totalPage ? (tensPage + 10 + 1) : totalPage);
            sb.Append("</li>");
        }

        sb.Append("</ul>");
        return sb.ToString();
    }
}
```

페이지네이션 디자인이 바뀌더라도 여기서 수정하면 되기 때문에 오픈소스를 이용하는 것보다 더 빠르게 능동적으로 대처할 수 있는 것 같다.

```
@if (Model.TotalCount != 0)
{
  @Html.Raw(Model.PaginationString())
}
```
cshtml에서는 리스트의 개수가 0개가 아닐 때, Pagination이 보이도록 한다.

```csharp
[AjaxOnly]
public async Task<PartialViewResult> Pv_GetArticleList(ArticleSearchDto articleSearchDto)
{
    articleSearchDto.AccountID = User.Identity.GetId();
    TypeListModel<Biz_ArticleList> model = await _db.GetArticleListAsync(articleSearchDto);
    return PartialView(model);
}
```

```csharp
query = query.AsNoTracking().OrderByDescending(x => x.CreateDate);
int total = await query.CountAsync();
int start = (articleSearchDto.Page - 1) * articleSearchDto.PageSize;
query = query.Skip(start).Take(articleSearchDto.PageSize);

PagingModel<Biz_ArticleList> data = new PagingModel<Biz_ArticleList>
{
    Source = await query.ToListAsync(),
    Page = articleSearchDto.Page,
    PageSize = articleSearchDto.PageSize,
    TotalCount = total
};

return data;
```

컨트롤러단과 서비스단이다.

페이지 디자인은 만들어졌지만 클릭하면 아무런 동작을 하지 않습니다.
페이지를 클릭했을 때, a링크를 넣는 경우 괜찮지만 그게 아닌 경우는 javascript 또는 jquery로 이벤트를 등록해주어야 합니다.

```javascript
$pageRoot.on('click', '#pager_list [data-page-value]', function () {
    var pVal = $(this).data('page-value');
    if (pVal > 0)
        page = pVal;
    else if (pVal === -1)
        page = 1;
    else if (pVal === -2) {
        if (page < 2)
            return false;
        page = page - 1;
    }
    else {
        if (totalPage <= page)
            return false;
        page = page + 1;
    }
    getPageList(); // ajax로 페이지 리스트를 호출하는 메서드
});
```