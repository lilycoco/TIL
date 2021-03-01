# React Lifecycle

## Reference

{% embed url="https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/" %}

{% embed url="https://ja.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html" %}

[componentWillUpdate](https://ja.reactjs.org/docs/react-component.html#unsafe_componentwillupdate) : &lt;deprecated&gt; Should be replaced to `componentDidUpdate`  
don’t use `setState`  
[componentWillMount](https://ja.reactjs.org/docs/react-component.html#unsafe_componentwillmount) : &lt;deprecated&gt; should be replaced to `componentDidMount`  
you can use `setState` but better use constructor as set initial value  
[componentWillReceiveProps](https://ja.reactjs.org/docs/react-component.html#unsafe_componentwillreceiveprops):  &lt;deprecated&gt;　should be replaced to `componentDidUpdate` , `getDerivedStateFromProps` or other methods  
you can use `setState` 

Where should I set states?  
setState\(\)

[constructor](https://ja.reactjs.org/docs/react-component.html#constructor): set state as initial value instead of `setState()`  
[componentDidMound](https://ja.reactjs.org/docs/react-component.html#componentdidmount) : you can use `setState`  
[componentDidUpdate](https://ja.reactjs.org/docs/react-component.html#componentdidupdate) : you can use `setState` with conditions 



