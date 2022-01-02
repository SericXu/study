## react-redux

```jsx
import {Provider} from "react-redux";

ReactDOM.render(
    /**
    * 	为了让app所有的自组件获得store，不需要单个添加
    */
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);

```

