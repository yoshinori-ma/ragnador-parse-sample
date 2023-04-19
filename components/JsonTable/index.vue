<template>
  <table v-if="json">
    <tbody>
    <tr v-for="[key, value] in data">
        <th>{{ key }}</th>
        <td>
            <template v-if="Array.isArray(value)">
                <JsonTable v-for="v in value" :json="v"></JsonTable>
            </template>
            <template v-else-if="value !== null && typeof value === 'object'">
                <JsonTable :json="value"></JsonTable>
            </template>
            <template v-else>{{ value }}</template>
        </td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
    props: {
        json: Object
    },
    computed: {
        data() {
            return Object.entries(this.json)
        }
    }
}

</script>