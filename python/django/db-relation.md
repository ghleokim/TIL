# DB relation

## 1:N relation
게시글-댓글의 형태로 1:N relation을 작성한다. **`1(Post)`:`N(Comment)`**

**Post**

| id(PK) | title | content | created_at | updated_at |
| ------ | ----- | ------- | ---------- | ---------- |
|        |       |         |            |            |

**Comment**

| id   | content | created_at | updated_at | post_id(FK) |
| ---- | ------- | ---------- | ---------- | ----------- |
|      |         |            |            |             |


### models.py: models 생성
`Post` 객체는 이와 같이 구성되어 있다.

```python
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

`Comments` 객체는 아래와 같이 구성된다. 이 때 `post` column에 `models.ForeignKey`를 넣어주고, 인자로는 `1`의 역할을 할 객체, `on_delete`에는 `models.CASCADE`를 넣어준다.

- `on_delete`: 1:N 관계에서 1이 삭제되었을 때의 behavior. 이 경우는 원 게시글이 사라졌을 때 댓글은 어떻게 할 것인가?
- 가장 좋은 관계는 1이 삭제되면 N도 삭제되는 것이다. django는 이러한 옵션을 위해 `models.CASCADE`를 가지고 있다.

```python
class Comment(models.Model):
    content = models.CharField(max_length=300)
    created_at = models.DatetimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    post = models.ForeignKey(Post, on_delete=models.CASCADE)
```

### urls.py

```python
urlpatterns = [
    ...
    path('<int:pk>/create_comment/', views.create_comment),
    path('<int:pk>/update_comment/<int:post_id>/', views.update_comment),
  	path('<int:pk>/delete_comment/<int:post_id>/', views.delete_comment),
]
```

### views.py

```python
def create_comment(request, pk):
  	return redirect()
def update_comment(request, pk):
  	return redirect()
def delete_comment(request, pk):
  	return redirect()
```

