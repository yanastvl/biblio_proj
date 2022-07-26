<template>
    <div class="task-inner">
        <div class="task">
            <div class="round" @click="toggleDone">
                <input :value="value.status" :checked="isDone" type="checkbox"/>
                <label for="checkbox"></label>
            </div>
            <div class="description">
                <p class="description-text">{{ value.description }}</p>
            </div>
            <div :class="`status status-${value.status}`">
                <p class="status-text">{{ taskStatus }}</p>
            </div>
            <div class="date">
                <p class="date-text">{{ taskDate }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import moment from 'moment';

export default {
    data() {
        return {
            search: require('../assets/img/search.svg'),
            isModalVisible: false,
            taskStatusMap: {
                "inProgress": "В работе",
                "complete": "Выполнено"
            },
        }
    },
    props: ['value'],
    computed: {
        taskStatus() {
            return this.taskStatusMap[this.value.status];
        },
        taskDate() {
            return moment(this.value.date).format('DD.MM.YYYY')
        },
        isDone() {
            return true ? this.value.status == "complete" : false
        },
        isDoneClass() {
            return 
        }
    },
    watch: {
        isDone() {
            return true ? this.value.status == "complete" : false
        },
    },
    methods: {
        toggleDone() {
            if (this.value.status === "complete") {
                this.value.status = "inProgress"
            } else {
                this.value.status = "complete"
            };
            localStorage.setItem(this.value.id, JSON.stringify(this.value));
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../scss/vars.scss';

.task-inner {
    border-top: 1px solid #EEEBE9;
}

.task{
    display: flex;
    flex-direction: row;
    align-items: center;

    max-width: 1300px;
    padding: 20px 0px;

    border-bottom: 1px solid #EEEBE9;

    &:hover{
        background: #F6F9FF
    }

    &:active{
        background: #F6F9FF
    }
}

.round{
    min-width: 6.1%;
}

.description{
    min-width: 72%;  
    padding-left: 20px;
}

.status{
    min-width: 11.6%; 
    padding-left: 20px;
}

.date{
    min-width: 10.3%; 
    padding-left: 20px;
}

.status-complete{
    color:$blue;
}

.status-inProgress{
    color:$orange;
}

.round {
    position: relative;
}

.round label {
    background-color: #fff;
    border: 1px solid $black;
    border-radius: 50%;
    cursor: pointer;
    height: 20px;
    position: absolute;
    width: 20px;
}

.round label:after {
    border: 1px solid $blue;
    border-top: none;
    border-right: none;
    content: "";
    height: 5px;
    left: 6px;
    opacity: 0;
    position: absolute;
    top: 5px;
    transform: rotate(-45deg);
    width: 8px;
}

.round input[type="checkbox"] {
    visibility: hidden;
    }

.round input[type="checkbox"]:checked + label {
    background-color: #fff;
    border-color: $blue;
}

.round input[type="checkbox"]:checked + label:after {
    opacity: 1;
}

</style>