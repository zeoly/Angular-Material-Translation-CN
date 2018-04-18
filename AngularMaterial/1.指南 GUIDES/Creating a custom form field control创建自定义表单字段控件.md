# 创建自定义表单字段控件

> Creating a custom form field control

现在可以实现在`<mat-form-field>`内部创建自定义表单字段控件了。如果你需要创建一个组件，用来与表单字段分享许多共同行为，这将是非常有用的，但是需要添加一些额外的的逻辑。

> It is possible to create custom form field controls that can be used inside `<mat-form-field>`. This can be useful if you need to create a component that shares a lot of common behavior with a form field, but adds some additional logic.

举个例子，在本指南中我们会学习怎样创建一个美式电话号码自定义输入框并把它与`<mat-form-field>`绑定。

> For example in this guide we'll learn how to create a custom input for inputting US telephone numbers and hook it up to work with `<mat-form-field>`. Here is what we'll build by the end of this guide:

让我们从一个可以在表单字段区域内起效的简单input组件开始，来学习如何创建自定义表单字段控件。例如，在手机号码输入时将数字在输入框中划分为几个片段。(注意:这并不是一个健壮的组件，只是我们学习的起点。)

> In order to learn how to build custom form field controls, let's start with a simple input component that we want to work inside the form field. For example, a phone number input that segments the parts of the number into their own inputs. (Note: this is not intended to be a robust component, just a starting point for us to learn.)

```ts
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

## 将组件作为MatFormFieldControl对外提供

> Providing our component as a MatFormFieldControl

第一步是将我们的新组件作为`MatFormFieldControl`接口的一个实现对外提供，则`<mat-form-field>`知道如何与其协作。为此，我们有自己的`MatFormFieldControl`实现类。由于这是一个泛型接口，所以我们需要引入一个类型参数，指示我们的控件将处理的数据类型，此例中是`MyTel`。然后我们向组件添加一个provider，这样就可将表单字段作为`MatFormFieldControl`进行注入了。

> The first step is to provide our new component as an implementation of the `MatFormFieldControl` interface that the `<mat-form-field>` knows how to work with. To do this, we will have our class implement `MatFormFieldControl`. Since this is a generic interface, we'll need to include a type parameter indicating the type of data our control will work with, in this case `MyTel`. We then add a provider to our component so that the form field will be able to inject it as a `MatFormFieldControl`.

```ts
@Component({
  ...
  providers: [{provide: MatFormFieldControl, useExisting: MyTelInput}],
})
class MyTelInput implements MatFormFieldControl<MyTel> {
  ...
}
```

设置组件后，就可以与`<mat-form-field>`协作了，但是现在我们还需要实现之前接口所声明的各种方法和属性。为学习更多`MatFormFieldControl`接口的知识，可以参见[表单字段API文档](https://material.angular.io/components/form-field/api)。

> This sets up our component so it can work with `<mat-form-field>`, but now we need to implement the various methods and properties declared by the interface we just implemented. To learn more about the `MatFormFieldControl` interface, see the [form field API documentation](https://material.angular.io/components/form-field/api).

## 实现MatFormFieldControl的方法和属性

> Implementing the methods and properties of MatFormFieldControl

### `value`

该属性允许我们设置或获取控件的值。它的类型与我们在实现`MatFormFieldControl`时使用的类型参数相同。因为我们的组件已经有了一个value属性，所以我们不需要为它写任何额外代码。

> This property allows someone to set or get the value of our control. Its type should be the same type we used for the type parameter when we implemented `MatFormFieldControl`. Since our component already has a value property, we don't need to do anything for this one.

### `stateChanges`

因为`<mat-form-field>`使用`OnPush`变化检测策略，当表单字段控件内部发生变化时，我们需要让它知道，可能需要表单字段来运行变更检测。我们可以通过`stateChanges`属性来实现它。到目前为止，表单字段唯一需要知道的是什么时候value会发生变化。当value发生变化时，我们需要在stateChanges流中发出事件，当我们持续清空这些属性时，我们可能会发现需要更多地方来发出事件。当组件被销毁时，要确保完成`stateChanges`事件。

> Because the `<mat-form-field>` uses the `OnPush` change detection strategy, we need to let it know when something happens in the form field control that may require the form field to run change detection. We do this via the `stateChanges` property. So far the only thing the form field needs to know about is when the value changes. We'll need to emit on the stateChanges stream when that happens, and as we continue flushing out these properties we'll likely find more places we need to emit. We should also make sure to complete `stateChanges` when our component is destroyed.

```ts
stateChanges = new Subject<void>();
set value(tel: MyTel | null) {
  ...
  this.stateChanges.next();
}
ngOnDestroy() {
  this.stateChanges.complete();
}
```

### `id`

这个属性会返回与`<mat-form-field>`关联的所有标签与提示的组件模板中元素的ID。在本例中，我们将使用属主元素并为它生成唯一的ID。

> This property should return the ID of an element in the component's template that we want the `<mat-form-field>` to associate all of its labels and hints with. In this case, we'll use the host element and just generate a unique ID for it.

```ts
static nextId = 0;
@HostBinding() id = `my-tel-input-${MyTelInput.nextId++}`;
```

### `placeholder`

这个属性允许我们控制`<mat-form-field>`如何使用占位符。在本例中，我们将使用`matInput`和`<mat-select>`做同样的事情，并允许用户通过`@Input()`指定它。由于占位符的值可能会随着时间的变化而变化，我们需要确保在占位符发生变更时，在父表单字段中发出`statechange`流来触发变更检测。

> This property allows us to tell the `<mat-form-field>` what to use as a placeholder. In this example, we'll do the same thing as `matInput` and `<mat-select>` and allow the user to specify it via an `@Input()`. Since the value of the placeholder may change over time, we need to make sure to trigger change detection in the parent form field by emitting on the `stateChanges` stream when the placeholder changes.

```ts
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

### `ngControl`

该属性允许表单字段控件指定一个`@angular/forms`控件绑定到此组件上。由于我们还没有设置我们的组件为`ControlValueAccessor`，所以我们在组件中设置为`null`。

> This property allows the form field control to specify the `@angular/forms` control that is bound to this component. Since we haven't set up our component to act as a `ControlValueAccessor`, we'll just set this to `null` in our component.

```ts
ngControl = null;
```

你需要先实现`ControlValueAccessor`后，组件才能与`formControl`和`ngModel`协作。实现`ControlValueAccessor`后，你需要获取与控件关联的`NgControl`的一个参考，并使其公共可用。

> It is likely you will want to implement `ControlValueAccessor` so that your component can work with `formControl` and `ngModel`. If you do implement `ControlValueAccessor` you will need to get a reference to the `NgControl` associated with your control and make it publicly available.


还有一个简单的方法是在构造体中将其作为一个公共属性添加进去，并让依赖注入操作它：

> The easy way is to add it as a public property to your constructor and let dependency injection handle it:

```ts
constructor(
  ..., 
  @Optional() @Self() public ngControl: NgControl,
  ...,
) { }
```

注意如果你的组件实现了`ControlValueAccessor`，有可能已经提供了`NG_VALUE_ACCESSOR`设置（在组件装饰器的`providers`部分，或者在模块的装饰中）。则可能获得到一个cannot instantiate cyclic dependency（无法实例化循环依赖）错误。

> Note that if your component implements `ControlValueAccessor`, it may already be set up to provide `NG_VALUE_ACCESSOR` (in the `providers` part of the component's decorator, or possibly in a module declaration). If so you may get a cannot instantiate cyclic dependency error.

可删除`NG_VALUE_ACCESSOR` provider并直接设置value accessor来解决此错误。

> To resolve this, remove the `NG_VALUE_ACCESSOR` provider and instead set the value accessor directly:

```ts
constructor(
  ..., 
  @Optional() @Self() public ngControl: NgControl,
  ...,
) {
  // Setting the value accessor directly (instead of using
  // the providers) to avoid running into a circular import.
  if (this.ngControl != null) { this.ngControl.valueAccessor = this; }
}
```

可参考[API文档](https://angular.io/api/forms/ControlValueAccessor)来获取更多关于`ControlValueAccessor`的信息。

> For additional information about `ControlValueAccessor` see the [API docs](https://angular.io/api/forms/ControlValueAccessor).

### `focused`

这个属性指示表单字段控件是否处于焦点状态。当它处于焦点状态时，表单字段显示为纯色下划线。对于组件来说，如果其中部分input处于焦点状态，则认为组件也处于焦点状态。我们可以使用`@angular/cdk`中的`FocusMonitor`来轻松地检查此状态。我们还需要记住在`statechange`流中发出的信息，这样就可以触发变更检测。

> This property indicates whether or not the form field control should be considered to be in a focused state. When it is in a focused state, the form field is displayed with a solid color underline. For the purposes of our component, we want to consider it focused if any of the part inputs are focused. We can use the `FocusMonitor` from `@angular/cdk` to easily check this. We also need to remember to emit on the `stateChanges` stream so change detection can happen.

```ts
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

### `empty`

这个属性指出表单字段控件是否是空的。对于我们的控件，如果所有部分都是空的，那么我们就认为它是空的。

> This property indicates whether the form field control is empty. For our control, we'll consider it empty if all of the parts are empty.

```ts
get empty() {
  let n = this.parts.value;
  return !n.area && !n.exchange && !n.subscriber;
}
```

### `shouldLabelFloat`

此属性用于指示标签是否应该在浮动位置。我们将使用与`matInput`相同的逻辑，当输入框在焦点状态或非空时,占位符将会浮动。由于占位符在没有浮动时将会覆盖我们的控制，所以当它没有浮动时，我们应该隐藏`–`字符。

> This property is used to indicate whether the label should be in the floating position. We'll use the same logic as `matInput` and float the placeholder when the input is focused or non-empty. Since the placeholder will be overlapping our control when when it's not floating, we should hide the `–` characters when it's not floating.

```ts
@HostBinding('class.floating')
get shouldLabelFloat() {
  return this.focused || !this.empty;
}
```

```css
span {
  opacity: 0;
  transition: opacity 200ms;
}
:host.floating span {
  opacity: 1;
}
```

### `required`

此属性用于指示是否input为必须。`<mat-form-field>`使用此信息向占位符添加一个必须的指示器。同样，我们要确保在需要的状态更改时运行更改检测。

> This property is used to indicate whether the input is required. `<mat-form-field>` uses this information to add a required indicator to the placeholder. Again, we'll want to make sure we run change detection if the required state changes.

```ts
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

### `disabled`

这个属性告诉表单字段在什么时候应该处于禁用状态。除了向表单字段报告正确的状态外，我们还需要在组成组件的单个input上设置禁用状态。

> This property tells the form field when it should be in the disabled state. In addition to reporting the right state to the form field, we need to set the disabled state on the individual inputs that make up our component.

```ts
@Input()
get disabled() {
  return this._disabled;
}
set disabled(dis) {
  this._disabled = coerceBooleanProperty(dis);
  this.stateChanges.next();
}
private _disabled = false;
```

```html
<input class="area" formControlName="area" size="3" [disabled]="disabled">
<span>–</span>
<input class="exchange" formControlName="exchange" size="3" [disabled]="disabled">
<span>–</span>
<input class="subscriber" formControlName="subscriber" size="4" [disabled]="disabled">
```

### `errorState`

这个属性指示关联的`NgControl`是否处于错误状态。因为我们在这个例子中没有使用`NgControl`，所以我们不需要做任何其他的事情，只需要将其设置为`false`。

> This property indicates whether the associated `NgControl` is in an error state. Since we're not using an `NgControl` in this example, we don't need to do anything other than just set it to `false`.

```ts
errorState = false;
```

### `controlType`

这个属性允许我们为表单字段中的控件类型指定一个唯一的字符串。`<mat-form-field>`将添加一个基于此类型的附加类，该类可用于轻松地将特殊样式应用于包含特定控件类型的`<mat-form-field>`中。在本例中，我们将使用`my-tel-input`作为我们的控制类型，这将导致表单字段添加`mat-form-field-my-tel-input`类。

> This property allows us to specify a unique string for the type of control in form field. The `<mat-form-field>` will add an additional class based on this type that can be used to easily apply special styles to a `<mat-form-field>` that contains a specific type of control. In this example we'll use `my-tel-input` as our control type which will result in the form field adding the class `mat-form-field-my-tel-input`.

```typescript
controlType = 'my-tel-input';
```

`setDescribedByIds(ids: string[])`

`<mat-form-field>`使用此方法来指定应该用于你的组件的`aria-describedby`属性的id。该方法有一个参数，id列表，我们只需要将给定的id应用到属主元素。

> This method is used by the `<mat-form-field>` to specify the IDs that should be used for the `aria-describedby` attribute of your component. The method has one parameter, the list of IDs, we just need to apply the given IDs to our host element.

```ts
@HostBinding('attr.aria-describedby') describedBy = '';
setDescribedByIds(ids: string[]) {
  this.describedBy = ids.join(' ');
}
```

### `onContainerClick(event: MouseEvent)`

当点击表单字段时会触发此方法。它允许你的组件钩入并处理此点击。该方法有一个用于点击的参数，`MouseEvent`。在此例中，如果用户没有点击任何`<input>`，那么焦点将在第一个`<input>`。

> This method will be called when the form field is clicked on. It allows your component to hook in and handle that click however it wants. The method has one parameter, the `MouseEvent` for the click. In our case we'll just focus the first `<input>` if the user isn't about to click an `<input>` anyways.

```ts
onContainerClick(event: MouseEvent) {
  if ((event.target as Element).tagName.toLowerCase() != 'input') {
    this.elRef.nativeElement.querySelector('input').focus();
  }
}
```

## 试一试

> Trying it out

现在我们已经完全实现了接口，准备好尝试我们的组件了!我们需要做的就是把它放到一个`<mat-form-field>`的内部。

> Now that we've fully implemented the interface, we're ready to try our component out! All we need to do is place it inside of a `<mat-form-field>`

```html
<mat-form-field>
  <my-tel-input></my-tel-input>
</mat-form-field>
```

我们还得到了`<mat-form-field>`的所有特性，如浮动占位符、前缀、后缀、提示和错误(如果我们已经给了表单字段一个`NgControl`并正确地报告错误状态)。

> We also get all of the features that come with `<mat-form-field>` such as floating placeholder, prefix, suffix, hints, and errors (if we've given the form field an `NgControl` and correctly report the error state).

```html
<mat-form-field>
  <my-tel-input placeholder="Phone number" required></my-tel-input>
  <mat-icon matPrefix>phone</mat-icon>
  <mat-hint>Include area code</mat-hint>
</mat-form-field>
```

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*