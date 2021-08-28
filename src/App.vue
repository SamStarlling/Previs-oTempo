import styled, {createGlobalStyle} from 'styled-components'
<template>
  <div id='app'>
  <GlobalStyle />
    <Div>
      <input type="text" placeHolder="...Procurando o país"/>
    </Div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      api_key: '02400b3ec5b08d0c3d23a6f587e55e1f'
    }
  }
}
</script>

const GlobalStyle = createGlobalStyle`
  margin: 50px;
  box-sizing: 0;
  padding: 0;
`
const Body = styled.body`
  font-family: 'Courier New', Courier, monospace;
`
const Div = styled.div`
 background-color: black;
`
