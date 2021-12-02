<template>
<n-space vertical size="large" class="space">
    <n-layout>
        <n-calendar @update:value="handleUpdateValue" #="{ year, month, date }" :value="value">
            <n-tag type="success" size="small" v-if="data[`${year}/${month}/${date}`]">{{data[`${year}/${month}/${date}`]}} KM</n-tag>
        </n-calendar>

        <n-statistic label="Statistic" :value="getMonthTotal()">
            <template #prefix>
                <n-icon>
                    <md-save />
                </n-icon>
            </template>
            <template #suffix>/ 300 KM</template>
        </n-statistic>

    </n-layout>

</n-space>

<n-modal v-model:show="showModal">
    <n-card style="width: 90vw;" :title="displayDate" :bordered="false" size="small">
        <!-- Content -->
        <n-input type="text" size="small" placeholder="Small Input" @keydown.enter="update()" v-model:value="modalInput" />
        <template #footer> Footer </template>
    </n-card>
</n-modal>
</template>

<script>
// import Calendar from './components/Calendar.vue'

export default {
    name: "app",
    data() {
        return {
            showModal: false,

            message: null,
            value: null,
            displayDate: null,

            modalInput: "",

            data: {
                "2021/12/2": 12.5,
                "2021/12/4": 15.5,
                "2021/12/7": 17.5,
                "2021/12/12": 16.5,
                "2021/12/16": 17.5
            }
        }
    },
    methods: {
        handleUpdateValue(isoDate, {
            year,
            month,
            date
        }) {
            this.value = isoDate
            this.displayDate = `${year}/${month}/${date}`
            this.showModal = true
            console.log(this.showModal)
        },
        update() {
            console.log(this.modalInput)
            this.data[this.displayDate] = parseInt( this.modalInput)
            this.modalInput = null
        },
        getMonthTotal() {
            return Object.entries(this.data).map(e => {
                const [
                    year,
                    month,
                    date
                ] = e[0].split("/")
                if (year == 2021 && month == 12) return e[1]
            }).reduce((a, b) => a + b, 0)
        }
    },
    mounted() {},
    components: {
        // Calendar
    }
}
</script>

<style>
.space {
    padding: 0.5rem
}

.n-calendar {
    height: auto;
}

.n-calendar .n-calendar-dates {
    flex: 0
}

.n-calendar .n-calendar-cell .n-calendar-date .n-calendar-date__day {
    position: absolute;
    bottom: 2.2rem;
}

.n-calendar .n-calendar-cell .n-calendar-date {
    justify-content: center;
}

.n-calendar .n-calendar-cell .n-calendar-date .n-calendar-date__date {
    margin: 0
}

.n-calendar .n-calendar-cell {
    padding: 10px 0;
    text-align: center;
}

.n-tag {
    padding: 0;
    text-align: center;
}

.n-tag__content {
    font-size: 8px;
    white-space: break-spaces;
}
</style>
