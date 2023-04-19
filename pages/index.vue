<template>
  <div>
      <form>
          <textarea v-model="yamlInput"></textarea>
      </form>

      <JsonTable :json="jsoned"></JsonTable>

      {{ common }}
  </div>
</template>
<script>
import jsyaml from 'js-yaml'
export default {
    data() {
        return {
            yamlInput: ''
        }
    },
    computed: {
        jsoned() {
            const doc = jsyaml.load(this.yamlInput, 'utf8')
            return doc
        },
        common() {
            if (!this.yamled) {
                return
            }

            return this.yamled['基本情報']
        }
    }
}
</script>