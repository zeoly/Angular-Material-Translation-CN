# 创建自定义表单字段控件

现在可以实现在`<mat-form-field>`内部使用的自定义表单字段控件了。如果你需要创建一个分享许多共同行为的表单字段，这将是非常有用的，但是需要添加一些额外的的逻辑。

> It is possible to create custom form field controls that can be used inside `<mat-form-field>`. This can be useful if you need to create a component that shares a lot of common behavior with a form field, but adds some additional logic.

举个例子，在本指南中我们会学习怎样创建一个美式电话号码自定义输入框并把它与`<mat-form-field>`挂起一起工作。

> For example in this guide we'll learn how to create a custom input for inputting US telephone numbers and hook it up to work with `<mat-form-field>`. 

为了学习如何构建自定义表单字段控件，让我们以一个简单的可以在表单字段区域内工作的输入框组件。例如，在手机号码输入时将数字在输入框中划分为几个片段。(注意:这并不是一个健壮的组件，只是我们学习的起点。)

> In order to learn how to build custom form field controls, let's start with a simple input component that we want to work inside the form field. For example, a phone number input that segments the parts of the number into their own inputs. (Note: this is not intended to be a robust component, just a starting point for us to learn.)

```typescript
class MyTel {
  constructor(public area: string, public exchange: string, public subscriber: string) {}
}
@Component({
  selector: 'my-tel-input',
  template: `
    <div [formGroup]="parts">
      <input class="area" formControlName="area" size="3">
      <span>&ndash;</span>
      <input class="exchange" formControlName="exchange" size="3">
      <span>&ndash;</span>
      <input class="subscriber" formControlName="subscriber" size="4">
    </div>
  `,
  styles: [`
    div {
      display: flex;
    }
    input {
      border: none;
      background: none;
      padding: 0;
      outline: none;
      font: inherit;
      text-align: center;
    }
  `],
})
class MyTelInput {
  parts: FormGroup;

  @Input()
  get value(): MyTel | null {
    let n = this.parts.value;
    if (n.area.length == 3 && n.exchange.length == 3 && n.subscriber.length == 4) {
      return new MyTel(n.area, n.exchange, n.subscriber);
    }
    return null;
  }
  set value(tel: MyTel | null) {
    tel = tel || new MyTel('', '', '');
    this.parts.setValue({area: tel.area, exchange: tel.exchange, subscriber: tel.subscriber});
  }

  constructor(fb: FormBuilder) {
    this.parts =  fb.group({
      'area': '',
      'exchange': '',
      'subscriber': '',
    });
  }
}
```

## 将我们提供的组件作为MatFormFieldControl

> Providing our component as a MatFormFieldControl

第一步：提供我们的新组件作为`MatFormFieldControl`接口的实现，并让`<mat-form-field>`知道怎样与其一同工作。为此，我们将实现类`MatFormFieldControl`。由于这是一个泛型接口，所以我们需要引入一个类型参数，指示我们的控件将处理的数据类型，在例子中是`MyTel`。然后我们向组件添加一个provider，这样表单字段就可以将它作为`MatFormFieldControl`注入了。

> The first step is to provide our new component as an implementation of the `MatFormFieldControl` interface that the `<mat-form-field>` knows how to work with. To do this, we will have our class implement `MatFormFieldControl`. Since this is a generic interface, we'll need to include a type parameter indicating the type of data our control will work with, in this case `MyTel`. We then add a provider to our component so that the form field will be able to inject it as a `MatFormFieldControl`.

```typescript
@Component({
  ...
  providers: [{provide: MatFormFieldControl, useExisting: MyTelInput}],
})
class MyTelInput implements MatFormFieldControl<MyTel> {
  ...
}
```

通过设置我们的组件，这样它就可以使用`<mat-form-field>`了，但是现在我们需要实现之前接口所声明的各种方法和属性。为学习更多`MatFormFieldControl`接口知识，参见[表单字段API文档](https://material.angular.io/components/form-field/api)。

> This sets up our component so it can work with `<mat-form-field>`, but now we need to implement the various methods and properties declared by the interface we just implemented. To learn more about the `MatFormFieldControl` interface, see the [form field API documentation](https://material.angular.io/components/form-field/api).

## 实现MatFormFieldControl的方法和属性


> Implementing the methods and properties of MatFormFieldControl

`value`

该属性允许某人设置或获取我们控件的值，它的类型应该与我们在实现`MatFormFieldControl`时使用的类型参数相同，因为我们的组件已经有了一个value属性，所以我们不需要为它写任何额外代码。

> This property allows someone to set or get the value of our control. Its type should be the same type we used for the type parameter when we implemented `MatFormFieldControl`. Since our component already has a value property, we don't need to do anything for this one.

`stateChanges`

因为`<mat-form-field>`使用`OnPush`变化检测策略，我们需要让它知道在表单字段控件中发生的事情。我们还可能需要表单字段来运行更改检测，我们可以通过`stateChanges`属性来实现它。到目前为止，表单字段惟一需要知道的是什么时候value会发生变化。当value发生变化时，我们需要在stateChanges流中发出，当我们持续冲洗这些属性时，我们可能会发现需要发出的更多地方。当我们的组件被销毁，们也要确保完成`stateChanges`。




> Because the `<mat-form-field>` uses the `OnPush` change detection strategy, we need to let it know when something happens in the form field control that may require the form field to run change detection. We do this via the `stateChanges` property. So far the only thing the form field needs to know about is when the value changes. We'll need to emit on the stateChanges stream when that happens, and as we continue flushing out these properties we'll likely find more places we need to emit. We should also make sure to complete `stateChanges` when our component is destroyed.

```typescript
stateChanges = new Subject<void>();
set value(tel: MyTel | null) {
  ...
  this.stateChanges.next();
}
ngOnDestroy() {
  this.stateChanges.complete();
}
```

`id`

这个属性会返回组件模板中元素的ID，这就是我们想要`<mat-form-field>`将其所有的标签和提示关联起来的结果。在本例中，我们将使用host元素并为它生成唯一的ID。

> This property should return the ID of an element in the component's template that we want the `<mat-form-field>` to associate all of its labels and hints with. In this case, we'll use the host element and just generate a unique ID for it.

```typescript
static nextId = 0;
@HostBinding() id = `my-tel-input-${MyTelInput.nextId++}`;
```

`placeholder`

这个属性允许我们告诉`<mat-form-field>`我们会在占位符提示中使用什么。这本例中，我们将会做与`matInput`和`<mat-select>`一样的的事情，允许用户通过`@Input()`指定它。由于占位符的值可能会随着时间的变化而变化，我们需要确保在父表单字段中通过在占位符更改时发出`statechange`流来触发变更检测。



> This property allows us to tell the `<mat-form-field>` what to use as a placeholder. In this example, we'll do the same thing as `matInput` and `<mat-select>` and allow the user to specify it via an `@Input()`. Since the value of the placeholder may change over time, we need to make sure to trigger change detection in the parent form field by emitting on the `stateChanges` stream when the placeholder changes.

```typescript
@Input()
get placeholder() {
  return this._placeholder;
}
set placeholder(plh) {
  this._placeholder = plh;
  this.stateChanges.next();
}
private _placeholder: string;
```

`ngControl`

该属性允许表单字段控件指定绑定到该组件的`@angular/forms`控件。由于我们还没有设置我们的组件作为`ControlValueAccessor`，所以我们在组件中设置为`null`。在任何实际组件中，您可能希望实现`ControlValueAccessor`，这样你的组件就可以使用`formControl`和`ngModel`。

> This property allows the form field control to specify the `@angular/forms` control that is bound to this component. Since we haven't set up our component to act as a `ControlValueAccessor`, we'll just set this to `null` in our component. In any real component, you would probably want to implement `ControlValueAccessor` so that your component can work with `formControl` and `ngModel`.

```typescript
ngControl = null;
```

如果你确实实现了`ControlValueAccessor`，您可以简单地注入`NgControl`并使之公开可用。(更多额外关于`ControlValueAccessor`的知识,可以查看[API文档](https://angular.io/api/forms/ControlValueAccessor))

> If you did implement `ControlValueAccessor`, you could simply inject the `NgControl` and make it publicly available. (For additional information about `ControlValueAccessor` see the [API docs](https://angular.io/api/forms/ControlValueAccessor).)

```typescript
constructor(..., @Optional() @Self() public ngControl: NgControl) { ... }
```

`focused`

这个属性指示表单字段控件是否处于焦点状态。当它处于焦点状态时，表单字段显示为纯色下划线。为了我们的组件的目的，我们想要考虑它的重点，如果任何部分的输入是集中的。我们可以使用`FocusMonitor`从`@angular/cdk`来轻松地检查这个。我们还需要记住在`statechange`流中发出的信息，这样就可以改变检测结果。


> This property indicates whether or not the form field control should be considered to be in a focused state. When it is in a focused state, the form field is displayed with a solid color underline. For the purposes of our component, we want to consider it focused if any of the part inputs are focused. We can use the `FocusMonitor` from `@angular/cdk` to easily check this. We also need to remember to emit on the `stateChanges` stream so change detection can happen.

```typescript
focused = false;
constructor(fb: FormBuilder, private fm: FocusMonitor, private elRef: ElementRef,
            renderer: Renderer2) {
  ...
  fm.monitor(elRef.nativeElement, renderer, true).subscribe(origin => {
    this.focused = !!origin;
    this.stateChanges.next();
  });
}
ngOnDestroy() {
  ...
  this.fm.stopMonitoring(this.elRef.nativeElement);
}
```

`empty`

这个属性指出表单字段控件是否是空的。对于我们的控件，如果所有部分都是空的，那么我们就认为它是空的。

> This property indicates whether the form field control is empty. For our control, we'll consider it empty if all of the parts are empty.

```typescript
get empty() {
  let n = this.parts.value;
  return !n.area && !n.exchange && !n.subscriber;
}
```

`shouldPlaceholderFloat`

此属性用于指示占位符是否应该在浮动位置。我们将使用与`matInput`相同的逻辑，当输入框在焦点状态或非空时,占位符将会浮动。由于占位符在没有浮动时将会覆盖我们的控制，所以当它没有浮动时，我们应该隐藏“-”字符。


> This property is used to indicate whether the placeholder should be in the floating position. We'll use the same logic as `matInput` and float the placeholder when the input is focused or non-empty. Since the placeholder will be overlapping our control when when it's not floating, we should hide the `–` characters when it's not floating.

```typescript
@HostBinding('class.floating')
get shouldPlaceholderFloat() {
  return this.focused || !this.empty;
}
span {
  opacity: 0;
  transition: opacity 200ms;
}
:host.floating span {
  opacity: 1;
}
```

`required`

此属性用于指示是否需要输入框。`<mat-form-field>`使用此信息向占位符添加必需的指示器。同样，我们要确保在需要的状态更改时运行更改检测。


> This property is used to indicate whether the input is required. `<mat-form-field>` uses this information to add a required indicator to the placeholder. Again, we'll want to make sure we run change detection if the required state changes.

```typescript
@Input()
get required() {
  return this._required;
}
set required(req) {
  this._required = coerceBooleanProperty(req);
  this.stateChanges.next();
}
private _required = false;
```

`disabled`

这个属性告诉表单字段在什么时候应该处于禁用状态。除了向表单字段报告正确的状态外，我们还需要在组成组件的单独输入框上设置禁用状态。

> This property tells the form field when it should be in the disabled state. In addition to reporting the right state to the form field, we need to set the disabled state on the individual inputs that make up our component.

```typescript
@Input()
get disabled() {
  return this._disabled;
}
set disabled(dis) {
  this._disabled = coerceBooleanProperty(dis);
  this.stateChanges.next();
}
private _disabled = false;
<input class="area" formControlName="area" size="3" [disabled]="disabled">
<span>&ndash;</span>
<input class="exchange" formControlName="exchange" size="3" [disabled]="disabled">
<span>&ndash;</span>
<input class="subscriber" formControlName="subscriber" size="4" [disabled]="disabled">
```

`errorState`

这个属性指示关联的`NgControl`是否处于错误状态。因为我们在这个例子中没有使用`NgControl`，所以我们不需要做任何其他的事情，只需要将其设置为`false`。

> This property indicates whether the associated `NgControl` is in an error state. Since we're not using an `NgControl` in this example, we don't need to do anything other than just set it to `false`.

```typescript
errorState = false;
```

`controlType`

这个属性允许我们为表单字段中的控件类型指定一个惟一的字符串。`<mat-form-field>`将添加一个基于此类型的附加类，该类可用于轻松地将特殊样式应用于包含特定控件类型的`<mat-form-field>`中。在本例中，我们将使用`my-tel-input`作为我们的控制类型，这将导致表单字段添加类`mat-form-field-my-tel-input`。


> This property allows us to specify a unique string for the type of control in form field. The `<mat-form-field>` will add an additional class based on this type that can be used to easily apply special styles to a `<mat-form-field>` that contains a specific type of control. In this example we'll use `my-tel-input` as our control type which will result in the form field adding the class `mat-form-field-my-tel-input`.

```typescript
controlType = 'my-tel-input';
```

`setAriaDescribedByIds(ids: string[])`

这个方法被`<mat-form-field>`使用，以指定应该用于你的组件的`aria-describedby`属性的id。该方法有一个参数，id列表，我们只需要将给定的id应用到主机元素。

> This method is used by the `<mat-form-field>` to specify the IDs that should be used for the `aria-describedby` attribute of your component. The method has one parameter, the list of IDs, we just need to apply the given IDs to our host element.

```typescript
@HostBinding('attr.aria-describedby') describedBy = '';
setDescribedByIds(ids: string[]) {
  this.describedBy = ids.join(' ');
}
```

`onContainerClick(event: MouseEvent)`

当单击表单字段时，将调用此方法。它允许你的组件钩入并处理它想要的点击。该方法有一个用于单击的参数`MouseEvent`。在我们的例子中，如果用户不打算单击`<input>`，那么我们将只关注第一个`<input>`。

> This method will be called when the form field is clicked on. It allows your component to hook in and handle that click however it wants. The method has one parameter, the `MouseEvent` for the click. In our case we'll just focus the first `<input>` if the user isn't about to click an `<input>` anyways.

```typescript
onContainerClick(event: MouseEvent) {
  if ((event.target as Element).tagName.toLowerCase() != 'input') {
    this.elRef.nativeElement.querySelector('input').focus();
  }
}
```

`Trying it out`

现在我们已经完全实现了接口，我们已经准备好尝试我们的组件了!我们需要做的就是把它放到一个`<mat-form-field>`的内部。

> Now that we've fully implemented the interface, we're ready to try our component out! All we need to do is place it inside of a `<mat-form-field>`

```html
<mat-form-field>
  <my-tel-input></my-tel-input>
</mat-form-field>
```

我们还得到了`<mat-form-field>`的所有特性，如浮动占位符、前缀、后缀、提示和错误(如果我们给表单字段一个`NgControl`并正确地报告错误状态)。

> We also get all of the features that come with `<mat-form-field>` such as floating placeholder, prefix, suffix, hints, and errors (if we've given the form field an `NgControl` and correctly report the error state).

```html
<mat-form-field>
  <my-tel-input placeholder="Phone number" required></my-tel-input>
  <mat-icon matPrefix>phone</mat-icon>
  <mat-hint>Include area code</mat-hint>
</mat-form-field>
```

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*