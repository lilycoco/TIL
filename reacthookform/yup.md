# yup

{% embed url="https://github.com/jquense/yup" %}

## 複数箇所の値比較

**mixed.oneOf\(arrayOfValues: Array&lt;any&gt;, message?: string \| function\): Schema Alias: equals**

**mixed.notOneOf\(arrayOfValues: Array&lt;any&gt;, message?: string \| function\)**

などで比較出来る

**yup.ref\(path: string, options: { contextPrefix: string }\): Ref**

を使うことで別箇所が変更された場合でも同時にvalidationを起動させることが出来る。

{% hint style="info" %}
How to validate one field matches another   
[https://github.com/jquense/yup/issues/49\#issuecomment-393100970](https://github.com/jquense/yup/issues/49#issuecomment-393100970)
{% endhint %}

`ref()`を使っても同時にvalidationを起動できない場合は、以下の実装を追加する

```javascript
  const linkedChallenge = watch("linkedChallenge");
  const language = watch("language");

  useEffect(() => {
    trigger("language");
    trigger("linkedChallenge");
  }, [trigger, linkedChallenge, language]);
```

{% hint style="info" %}
ReactHookForm \#trigger  
[https://react-hook-form.com/jp/api\#trigger](https://react-hook-form.com/jp/api#trigger)
{% endhint %}

### イレギュラーな実装

#### 指定する`PathName`が指定元の`PathName`と同じ場合

複数箇所の値を比較してvalidation設定をする際、指定する`PathName`が指定元の`PathName`と同じ場合、`PathName`を指定しても指定元自体を示してしまい、上手くValidationが設定できない。以下のAPIを使うことで指定する`PathName`が指定元の`PathName`と同じだった場合でも値を比較することが出来る。

**mixed.test\(name: string, message: string \| function, test: function\): Schema**

```javascript
  const schema = yup.object().shape({
    language: yup
      .string()
      .required()
      .notOneOf([yup.ref("linkedChallenge.language")]),
    linkedChallenge: yup
      .object()
      .test(
        "notEqual",
        MessageService.getMessageByKey("validation.string.linkedChallenge"),
        function () {
          return this.parent.language !== this.parent.linkedChallenge.language;
        },
      ),
  });
```

{% hint style="info" %}
Test functions are called with a special context, or `this` value, that exposes some useful metadata and functions. Older versions just expose the `this` context using `function ()`, not arrow-func, but now it's exposed too as a second argument of the test functions. It's allow you decide which approach you prefer.

* `this.path`: the string path of the current validation
* `this.schema`: the resolved schema object that the test is running against.
* `this.options`: the `options` object that validate\(\) or isValid\(\) was called with
* `this.parent`: in the case of nested schema, this is the value of the parent object
* `this.originalValue`: the original value that is being tested
* `this.createError(Object: { path: String, message: String, params: Object })`: create and return a validation error. Useful for dynamically setting the `path`, `params`, or more likely, the error `message`. If either option is omitted it will use the current path, or default message.
{% endhint %}



