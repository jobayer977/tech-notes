# Angular Component Architecture

In Angular components are internally treated as view data. View-Data is a data structure that holds information about components.

## Example of Independent Component

Let's say we have a component:

```typescript
@Component({
	selector: 'app-root',
	template: `<h1>Hello World</h1>`,
})
export class AppComponent {}
```

Angular compiler First compiles the template(HTML) and creates a view definition. View definition represents data about tags and bindings within the template. After that create a View data structure from the view definition that holds information about the template and how it should be rendered. Look like this:

```typescript
{
    component: new AppComponent(),
    nodes: [
        {
            renderElement:'h1',
            text: 'Hello World'
        }
    ],
    def:{}
}

```

After that, it will be mapping the view definition with browser DOM. And then create a DOM element for each node in the view definition. And then it will be rendered in the browser. Look like this:

```html
<h1>Hello World</h1>
```

```output
Hello World
```

This is how a basic component is rendered in the browser.

## Example of Nested Component

In composable component. We can use one component inside another component. Let's say we have a component:

```typescript
@Component({
	selector: 'app-hello',
	template: `<h1>Hello World</h1>`,
})
export class HelloComponent {}
```

And we have another component:

```typescript
@Component({
	selector: 'app-root',
	template: `
		<h1>App Component</h1>
		<app-hello></app-hello>
	`,
})
export class AppComponent {}
```

Like Before angular first compiles and creates a view definition and view data structure.

**Hello Component View Definition**

```typescript
{
    component: new HelloComponent(),
    nodes: [
        {
            renderElement:'h1',
            text: 'Hello World'
        }

    ],
}
```

**App Component View Definition**

```typescript
{
    component: new AppComponent(),
    nodes: [
        {
            renderElement:'h1',
            text: 'App Component'
        },
        {
            def:HelloComponent
        }
    ],
}
```

So, in the composable component, we can see that the view definition of the child component is added to the parent component view definition. And then it will be mapped to the DOM element as well.
