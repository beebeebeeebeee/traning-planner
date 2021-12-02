<template>
<n-calendar @update:value="handleUpdateValue" #="{ year, month, date }" v-model:value="value" :is-date-disabled="isDateDisabled">
    {{ year }}-{{ month }}-{{ date }}
</n-calendar>
</template>

<script>
import {
    defineComponent,
    ref
} from 'vue'
import {
    useMessage
} from 'naive-ui'
import {
    isYesterday,
    addDays
} from 'date-fns'

export default defineComponent({
    setup() {
        const message = useMessage()
        return {
            value: ref(addDays(Date.now(), 1).valueOf()),
            handleUpdateValue(_, {
                year,
                month,
                date
            }) {
                message.success(`${year}-${month}-${date}`)
            },
            isDateDisabled(timestamp) {
                if (isYesterday(timestamp)) {
                    return true
                }
                return false
            }
        }
    }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
