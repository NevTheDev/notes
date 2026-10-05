# .NET MAUI

## Data Binding

```html
<Label Text="{Binding PropertyName}" />
```

Binding using a view model (MVVM) we need to add a namespace declaration that points to the namespace of the viewmodel.

We also need to tell the page what the binding context is.

### xaml only

``` xml
<ContentPage ...
    xmlns:vm="clr-namespace:ViewModels.Namesapce"
    ...
    >
    <ContentPage.BindingContext>
        <vm:NameOfViewModel />
    </ContentPage.BindingContext>
    ...
```

### Data Type

``` xml
<ContentPage ...
    xmlns:vm="clr-namespace:ViewModels.Namesapce"
    x:DataType="vm:NameOfViewModel"
    ...
>
```

```c#
 public MyContentPage(SomeViewModel model)
 {
     InitializeComponent();
     BindingContext = model;
 }
```

Pages can also be bound to them selfs.

```c#
 public MyContentPage()
 {
     InitializeComponent();
     BindingContext = this;
 }
```