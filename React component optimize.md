# React component optimize

> (Presentational and Container Components)[https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0]

1 presentational components:
- Are concerned with how things look.
- May contain both presentational and container components** inside, and usually have - some DOM markup and styles of their own.
- Often allow containment via this.props.children.
- Have no dependencies on the rest of the app, such as Flux actions or stores.
- Don’t specify how the data is loaded or mutated.
- Receive data and callbacks exclusively via props.
- Rarely have their own state (when they do, it’s UI state rather than data).
- Are written as functional components unless they need state, lifecycle hooks, or - - - performance optimizations.

2 container components
- Are concerned with how things work.
- May contain both presentational and container components** inside but usually don’t - have any DOM markup of their own except for some wrapping divs, and never have any - - styles.
- Provide the data and behavior to presentational or other container components.
- Call Flux actions and provide these as callbacks to the presentational components.
- Are often stateful, as they tend to serve as data sources.
- Are usually generated using higher order components such as connect() from React Redux, createContainer() from Relay, or Container.create() from Flux Utils, rather than written by hand.

## Benefits of This Approach
- Better separation of concerns. You understand your app and your UI better by writing components this way.
- Better reusability. You can use the same presentational component with completely different state sources, and turn those into separate container components that can be further reused.
- Presentational components are essentially your app’s “palette”. You can put them on a single page and let the designer tweak all their variations without touching the app’s logic. You can run screenshot regression tests on that page.
- This forces you to extract “layout components” such as Sidebar, Page, ContextMenu and use this.props.children instead of duplicating the same markup and layout in several container components.