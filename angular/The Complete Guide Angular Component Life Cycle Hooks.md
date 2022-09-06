Angular has 8 lifecycle hooks. They are:

1. ngOnChanges
2. ngDoCheck
3. ngOnInit
4. ngAfterContentInit
5. ngAfterContentChecked
6. ngAfterViewInit
7. ngAfterViewChecked
8. ngOnDestroy

## ngOnChanges

This is called whenever there is a change in the input properties of a component. It receives a SimpleChanges object of current and previous property values.

**Example:**

**Parent Component**

```typescript
@Component({
	selector: 'app-parent',
	template: `
		<h1>Parent Component</h1>
		<app-child [name]="name"></app-child>
	`,
})
export class ParentComponent {
	name = 'Angular'
}
```

**Child Component**

```typescript
@Component({
	selector: 'app-child',
	template: `
		<h3>Child Component</h3>
		<p>Message: {{ message }}</p>
	`,
})
export class ChildComponent implements OnChanges {
	@Input() message: string
	ngOnChanges(changes: SimpleChanges) {
		for (const propName in changes) {
			const change = changes[propName]
			const from = JSON.stringify(change.previousValue)
			const to = JSON.stringify(change.currentValue)
			console.log(`${propName} changed from ${from} to ${to}`)
		}
	}
}
```

In The above example whenever the name property of the parent component changes, the `ngOnChanges` method of the child component will be called.

## ngDoCheck

This is called during every change detection run, immediately after ngOnChanges() and ngOnInit(). It is invoked in every change detection cycle anywhere on the page.

**Example:**

```typescript
@Component({
	selector: 'app-child',
	template: `
		<h3>Child Component</h3>
		<p>Message: {{ message }}</p>
	`,
})
export class ChildComponent implements DoCheck {
	@Input() message: string
	ngDoCheck() {
		console.log('ngDoCheck called')
	}
}
```

In the above example, the `ngDoCheck` method of the child component will be called in every change detection cycle. like when the message property of the parent component changes.

**Note:** Use this hook to extend change detection by performing a custom check. For example, you can check if the values of two properties are not equal and then set the third property when they are not equal.

## ngOnInit

This is called after the constructor, Whenever Angular initializes the directive or component after creating it. It is only called once when the component is initialized.

**Example:**

```typescript
@Component({
	selector: 'app-hello',
	template: ` <h3>hello Component</h3> `,
})
export class HelloComponent implements OnInit {
	constructor() {
		console.log('constructor called')
	}
	ngOnInit() {
		console.log('ngOnInit called')
	}
}
```

In the above example, the `ngOnInit` method of the hello component will be called after the constructor.

## ngAfterContentInit

This is called after Angular projects external content into the component's view or the view that a directive is in. It is called only once.

**Example:**

```typescript
@Component({
	selector: 'app-child',
	template: `
		<h3>Child Component</h3>
		<ng-content></ng-content>
	`,
})
export class ChildComponent implements AfterContentInit {
	constructor() {
		console.log('constructor called')
	}
	ngAfterContentInit() {
		console.log('ngAfterContentInit called')
	}
}
```

```typescript
@Component({
	selector: 'app-parent',
	template: `
		<h1>Parent Component</h1>
		<app-child>
			<p>Message: {{ message }}</p>
		</app-child>
	`,
})
export class ParentComponent {
	message = 'Angular'
}
```

When the parent component is rendered, the `ngAfterContentInit` method of the child component will be called.

## ngAfterContentChecked

This is called after every check of the component's or directive's content. It is invoked in every change detection cycle anywhere on the page.

**Example:**

```typescript
@Component({
	selector: 'app-child',
	template: `
		<h3>Child Component</h3>
		<ng-content></ng-content>
	`,
})
export class ChildComponent implements AfterContentChecked {
	constructor() {
		console.log('constructor called')
	}
	ngAfterContentChecked() {
		console.log('ngAfterContentChecked called')
	}
}
```

```typescript
@Component({
	selector: 'app-parent',
	template: `
		<h1>Parent Component</h1>
		<app-child>
			<p>Message: {{ message }}</p>
		</app-child>
	`,
})
export class ParentComponent {
	message = 'Angular'
}
```

When the parent component is rendered, the `ngAfterContentChecked` method of the child component will be called in every change detection cycle. like when the message property of the parent component changes.
