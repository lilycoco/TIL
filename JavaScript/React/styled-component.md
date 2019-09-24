
# styled-component 💅

https://www.vietnguyen.site/getting-started-with-nextjs-styled-components-and-rebass/

```
import styled from 'styled-components'

const Flex = styled(div)`
    display: flex;
    ${props => (props.boxShadow ? `box-shadow: ${props.boxShadow};` : null)}
    ${props => (props.height ? `height: ${props.height};` : null)}
    ${props => (props.center ? `justify-content: center; align-items: center;` : null)}
    ${props => (props.border ? `border: ${props.border};` : null)}
`

export default props => (
    <Flex {...props}>
        {props.children}
    </Flex>
)
```
