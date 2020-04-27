#### Description
a vue component of smart tips input box.
cotain two components: {TgSmartInput, BaseTag}

#### install
npm install --save-dev tg-smart-input

#### import
import TgSmartInput from "tg-smart-input"

#### use
```js
/*app.vue*/
<template>
<tg-smart-input 
  :multiple="multiple"
  :data-api="dataApi"
  :key-name="keyName"
  ref="smartInput"
/>

<base-tag 
  closeable
  size="small"
  type="info"
  effect="light"
  bgColor="yellow"
>
{{ msg }}
</base-tag>
</template>
<script>
import TgSmartInput from 'tg-smart-input';

export default {
  name: 'App',
  data() {
    return {
      msg: 'test',
      multiple: true,
      dataApi: 'http://tgtest.local/members/members?q=',
      keyName: 'zhongwen',
      //members: [{"name":"john","zhongwen": "小强"},{"name":"allan","zhongwen": "老王"},{"name":"krwakg","zhongwen": "老吴"}],
    }
  },
  components: {
    TgSmartInput: TgSmartInput.TgSmartInput,    // register component tg-smart-input
    BaseTag: TgSmartInput.BaseTag
  }
}
</script>
<style></style>
```