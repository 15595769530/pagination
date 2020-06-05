# 快速分页功能



## 页面效果

![image-20200605120655162](C:\Users\evan\AppData\Roaming\Typora\typora-user-images\image-20200605120655162.png)

![image-20200605120711075](C:\Users\evan\AppData\Roaming\Typora\typora-user-images\image-20200605120711075.png)

### 使用示例

**Django视图**

```
def issues(request, project_id):
    if request.method == "GET":
        queryset = Issues.objects.filter(project_id=project_id)
        page_object = Pagination(
            current_page=request.GET.get('page'),
            all_count=queryset.count(),
            base_url=request.path_info,
            query_params=request.GET,
        )
        object_list = queryset[page_object.start:page_object.end]
        form = IssuesModelForm(request)
        context = {
            'issues_object_list': object_list,
            'page_html': mark_safe(page_object.page_html())

        }

        return render(request, 'issues.html', {'context': context})
```

**前端html**

```
<nav aria-label="...">
    <ul class="pagination" style="margin-top: 0;">
      {{ context.page_html }}
    </ul>
</nav>
```
