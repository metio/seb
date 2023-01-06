---
title: Declarative conditional rendering in React
date: 2022-01-15
menu: topic
categories:
- snippet
- frontend
tags:
- react
- rendering
- breakpoints
---

One feature that often surprises people while teaching them [React](https://reactjs.org/) is that a component does not have to render anything. It seems trivial at first, however it quickly shows that a render-nothing components can reduce boilerplate code and improve code-reuse.

In its simplest (shortest) form a render-nothing component looks like the following snippet. It does not actually do anything and is not particularly helpful for anything. You could add it to every other component in your application without breaking or influencing anything.

```jsx
const RendersNothing = () => <></>
```

Now consider the following example, that adds some `if-then-else` logic to the same component:

```jsx
const MightRenderSomething = () => {
  if (someCondition) {
    return <span>hello world!</span>
  }
  return <></>
}
```

This component encapsulates the `if-then-else` logic of conditionally rendering a hello world message. Instead of cluttering your entire app with the same logic, you can now simply re-use that same component that contains this `if` condition. In order to see the full power of this technique, consider the following example. At first, we are going to define a hook that reads the current window width, then define components that conditionally render based on the current window width, and finally use those components in an example application.

```jsx
const useWindowWidth = () => {
  const [width, setWidth] = React.useState(0)

  React.useEffect(() => {
    const handleResize = () => {
      setWidth(window.innerWidth)
    }
    window.addEventListener("resize", handleResize)
    return () => {
      window.removeEventListener("resize", handleResize)
    }
  }, [])

  return width
}
```

The following components use that hook in order to implement UI breakpoints for small (mobile) and large (desktop) screens. Note that the value `768` is just an example - replace it with whatever your design system tells you to.

```jsx
const ForMobileDevicesOnly = (props) => {
  const windowWidth = useWindowWidth()
  
  if (windowWidth < 768) {
    return <>{props.children}</>
  }
  return <></>
}

const ForDesktopDevicesOnly = (props) => {
  const windowWidth = useWindowWidth()

  if (windowWidth >= 768) {
    return <>{props.children}</>
  }
  return <></>
}
```

Both of these components simply render nothing when the window width does not have an appropriate size. If the window width does have the right size, they render their `children`. We can use those components in our application like this:

```jsx
const SomeActualComponent = () => (
    <div>
      <h1>common headline</h1>
      <ForMobileDevicesOnly>
        <span>only visible on mobile devices</span>
      </ForMobileDevicesOnly>
      <ForDesktopDevicesOnly>
        <span>only visible on desktop devices</span>
      </ForDesktopDevicesOnly>
    </div>
)
```

The above code snippet declares that some part of the UI can only be seen by mobile users, while others can only be seen by desktop users. Parts of the UI that are shared amongst all users are not wrapped by any of the components defined above.
