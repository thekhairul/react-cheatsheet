### Pass props

```html
<Video fullscreen={true} autoplay={false} />
```

### Recieve props
```jsx
// Video component
render () {
  const { fullscreen, autoplay } = this.props
  ···
}
```

Use `this.props` to access properties passed to the component.

See: [Properties](https://reactjs.org/docs/tutorial.html#using-props)