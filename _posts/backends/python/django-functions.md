---
title: 在 Django 中的几个方法
abbrlink: 494804a0
date: 2021-05-10 14:11:02
updated: 2021-07-12 14:11:02
tags:
    - django
    - template
categories:
    - 后台技术
    - Python
    - Django
keywords:
    - Django
    - Template
group: notes
description: 如何在 Django 中模板传入额外的字段值。
---

## extra_context

模板中增加一个按钮字段：

```html django模板
<div>
  {{ button_name }}
</div>
```

在向模板中传入一个字段值的时候，如果 只是在模板中使用，可以利用 `extra_context` 。

```python urls.py
views.ClassView.as_view(extra_context={'button':'button_name'})
```

## get_context_data()

```python views.py
class PrCreateView(CreateView):
    ...
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['button'] = 'button_name'
        
        return context
```


两个方法都可以将按钮名传到模板中，不同的是一个是在 url 中定义的，另一个是在类的 `get _context_data` 方法，各有优势吧。

## get_queryset()

该方法可以返回一个量身定制的对象列表。可以通过调整返回一个更符合预期的对象列表。

示例如下，主要是按用户进行了一次排序，将排序后的数据，传递给类视图。

```python
def get_queryset(self):
    self_objectives = UserObjective.objects.filter(
        user=self.request.user.user_profile)
    other_objectives = UserObjective.objects.exclude(
        user=self.request.user.user_profile)
    
    objectives = list(self_objectives) + list(other_objectives)
    return objectives
```

## get_context_data()

适用于给模板传递模型以外内容或者参数，比如增加一个特殊的内容，可以重写该方法来实现。

示例如下，这里是针对传入的 context 又增加了一个 now 的字段：

```python
def get_context_data(self, **kwargs):
    context = super().get_context_data(**kwargs)
    context['now'] = timezone.now()
    return context
   ```
## get_object()

通过从 URL 获取根据 pk 或其它参数调取一个对象来进行后续操作，可以通过此方法为对象的内容进行筛选，用此获取更具体的对象。

示例如下，这里是返回此用户有权限的对象，通过这种方式可以控制用户只能查看编辑与自己相关的内容。

```python
def get_object(self, queryset=None):
       obj = super().get_object(queryset=queryset)
       if obj.author != self.request.user:
              raise Http404()
       return obj
```
