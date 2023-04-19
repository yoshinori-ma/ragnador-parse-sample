<template>
  <div>
      <form>
          <VTextarea label="コピペしたやつはりつけるとこ" v-model="yamlInput"></VTextarea>
      </form>
<!--      <JsonTree :data="jsoned"></JsonTree>-->
      <template v-if="jsoned">
          <div v-for="(value, key) in countedTsukumo">
              <div>{{ key }}</div>
              <div>{{ value.join(',') }}</div>
          </div>
      </template>

      <JsonTable :json="jsoned"></JsonTable>
  </div>
</template>
<script>
import jsyaml from 'js-yaml'
import JsonTree from 'vue-json-tree'
import _ from 'lodash'
export default {
    components: {
        JsonTree
    },
    data() {
        return {
            yamlInput: ''
        }
    },
    methods: {
        groupBy: _.groupBy
    },
    computed: {
        groupedTsukumoName() {
          return this.groupBy(this.jsoned['ツクモ情報'], '名前')
        },
        countedTsukumo() {
            return Object.fromEntries(Object.entries(this.groupedTsukumoName).map(([k,v]) => {
                return [k,Object.entries(_.groupBy(v, (value) => `${value['グレード']}#${value['覚醒値']}`)).map(([grade, array]) => `${grade}:${array.length}`)]
            }))
        },
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