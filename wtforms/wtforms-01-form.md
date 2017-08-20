# Form
Form 提供了WTForms中最高级别的API。 包含了对field的定义，委托验证，输入，汇集错误，一般来说还可以作为将所有东西联结起来的胶水程序。

## Form类
```python
class wtforms.form.Form
__init__(formdata=None, obj=None, prefix='', data=None, meta=None, **kwargs)
```

### 参数
+ formdata - 用来传输来自用户的数据，通常需要 request.POST 或类似的东西。 formdata 是一个可以通form输入获得多个变量的request-data包裹函数，变量包括 unicode 字符串。
+ obj - 如果没有formdata，这个obj对象会用来匹配并用form字段名，
+ prefix - 提供prefix的话，所有field会以其值为字段名前缀
+ data - 提供一个dict类型的数据。这个参数只在formdata和obj都没有提供的时候使用
+ meat - 看不懂
+ **kwargs - 如果没有formdata，并且obj没有与field匹配成功，form将把字典中的值分配给相对应的field

初始化一个Form一般是在你的程序中一个视图函数或是控制器的上下文中完成的。但一个Form构建完成，field会根据formdata，obj，kwargs获得值。

*提示：* 必须为后备储存对象和kwargs提供已经强制格式化过的数据类型。WTForms不会检查这些值或将他们格式化成为正好是formdata需要的，因为预期此数据是来自于form所代表的支持存储库的默认值或数据。查看      usingforms  了解更多

### 属性
+ data 
    * 一个包含每一个field数据的dict
    * 注意，每次访问这个属性都会生成一次，所以要注意什么时候使用它，因为如果经常重复重复访问它的代价是比较大的。你可以在你需要迭代所form中的数据的时候使用（因form中的field不可以直接访问）。如果你仅仅是需要访问已知可见的field，你应该使用form.\<field\>.data.
+ errors
    * 一个包含每一个field一系列错误的dict，如果它是空的，那form要么没有被验证通过，要么就是没有错误。 （为毛没有验证通过是空的？）
    * 注意， 这个属性并不活跃，只有当你首次访问它的时候会生成，如果你在访问它之后执行validate()，缓存中的结果会失效，并且在下一次访问的时候重新生成。
+ meta 这个对象包含着一系列可自定form行为的配置选项。查看 class Meta

### 方法
+ validate()
    * 通过在每一个fiedl调用validate()来验form， passing any extra Form.validate_<fieldname> validators to the field validator.（看不懂）
+ populate_obj(obj)
    * obj中的参数会被form中同名的field重写。
    * 注意，这是一个有害的方法。任何有与field同名的变量将会被重写，谨慎使用
      一个普遍的用法是修改资料的界面
    ```python
    def edit_profile(request):
    user = User.objects.get(pk=request.session['userid'])
    form = EditProfileForm(request.POST, obj=user)

    if request.POST and form.validate():
        form.populate_obj(user)
        user.save()
        return redirect('/home')
    return render_to_response('edit_profile.html', form=form)
    ```
    在这个例子中，因为form不直接与user对象直接关联，所以你不用担心有任何表单中传递来的不好的数据写入。直到你准备好删除它。

+ __iter__()
    * 以创建的顺序迭代form中的field
    ```html
    {% for field in form %}
        <tr>
            <th>{{ field.label }}</th>
            <td>{{ field }}</td>
        </tr>
    {% endfor %}
    ```

+ __contains__(name)
    * 如果命名了的字段是这个form中之一，返回True

## 定义表单
要定义一个表单，你可以定义一个Form的子类，并定义类的属性，作为field
```python
class MyForm(Form):
    first_name = StringField(u'First Name', validators=[validators.input_required()])
    last_name  = StringField(u'Last Name', validators=[validators.optional()])
```
Field 的name 可以是任何python有效的标识符，限制为以下几点

+ 大小写敏感
+ 不能以下划线为首字母
+ 不能以"validate"开头

### Form 的继承

Form类可以根据需要定义子类。子类Form会包含所有父类的field，和子类中新定义的新field。子类中新的field 如果与父类中的旧field重名会导致旧的掩盖旧的。

```python
class PastebinEdit(Form):
    language = SelectField(u'Programming Language', choices=PASTEBIN_LANGUAGES)
    code     = TextAreaField()

class PastebinEntry(PastebinEdit):
    name = StringField(u'User Name')
```

### 内部validator

为了给提供用户临时使用的validator，可以在类中定义内部validator方法，方法名为validate_<field名>这样就不必为了仅仅使用一次的validator特意写一个。

```python
class SignupForm(Form):
    age = IntegerField(u'Age')

    def validate_age(form, field):
        if field.data < 13:
            raise ValidationError("We're sorry, you must be 13 or older to register")
```



### Form的使用

form一般是在控制器代码中构建，构造函数中的数据来自从框架中传递来的包装器方法，也可以有一个ORM对象。下面是一个典型的用法

```python
def edit_article(request):
    article = Article.get(...)
    form = MyForm(request.POST, article)
```

一个典型的CRUD（增删改查）界面函数