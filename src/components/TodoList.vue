<template>
    <div class="container">
        <div class="main">
            <h1 class="main-caption">To do list</h1>
            <ButtonPlus @showModal="showModal"/>
        </div>
        <div class="navigation">
            <div class="search-inner">
                <img class="img-search" :src="search" alt=""/>
                <Input
                type="text" 
                class="input-search"
                name="query" 
                :model="query"
                @keyup="filter"
                placeholder="Поиск ID, Имени, статуса или даты"
                ></Input> 
            </div>
            <div class="sorting">
                <p class="sorting-text">Сортировать по:</p>
                <select class="sorting-inner" @change="sort" v-model="sorting">
                    <option value="date">Дата</option>
                    <option value="status">Статус</option>
                </select>

            </div>
        </div>
        <div class="todo-inner">
            <div class="todo-header">
                <div class="checkbox-col"></div>
                <div class="line"></div>
                <div class="description-col">
                    <p>Описание</p>
                </div>
                <div class="line"></div>
                <div class="status-col">
                    <p>Статус</p>
                </div>
                <div class="line"></div>
                <div class="date-col">
                    <p>Дата</p>
                </div>
            </div>
            <div v-for="(task, index) in tasks" :key="task.id">
                <Task v-model="tasks[index]" @update="initialize"></Task>
            </div>
        </div>
      <ModalWindow :visible="isModalVisible" @hideModal="hideModal" @update="initialize"/>
    </div>
</template>

<script>
import ButtonPlus from './ButtonPlus.vue';
import Task from './Task.vue';
import ModalWindow from './ModalWindow.vue';

export default {
    data() {
        return {
            search: require('../assets/img/search.svg'),
            isModalVisible: false,
            query: '',
            tasks: [],
            sorting: 'date',
        }
    },
    created() {
        this.initialize();
        this.sort();
    },
    methods: {
        initialize(filteredTasks) {
            const tasks = [];
            Object.keys(localStorage).forEach(function(key){
                tasks.push(JSON.parse(localStorage.getItem(key)));
            });
            this.tasks = filteredTasks ? filteredTasks : tasks;
        },
        showModal() {
            this.isModalVisible = true;
        },
        hideModal() {
            this.isModalVisible = false;
        },
        filter(e) {
            this.initialize();
            const filteredTasks = this.tasks.filter(res => {
                return(JSON.stringify(res).toLocaleLowerCase()).match(e.target.value.toLocaleLowerCase());
            })
            this.initialize(filteredTasks);
        },
        sort() {
            this.tasks.sort((a, b) => {
                if ( a[this.sorting] > b[this.sorting] ){ return -1;}
                if ( a[this.sorting] < b[this.sorting] ){ return 1;}
                return 0;
            })
        }
    },
    components: { ButtonPlus, Task, ModalWindow}
};
</script>

<style lang="scss" scoped>

.container{
    width: 100%;
    margin: auto;
    max-width:100rem;
    padding: 104px 150px;
}

.main, .navigation{
    display: flex;
    justify-content:space-between;
}

.navigation{
    padding-top: 30px;
}

.search-inner{
    display: flex;
    align-items: center;
}

.main-caption{
    font-family: 'Montserrat', sans-serif;
    font-weight: 700;
    font-size: 24px;
    line-height: 32px;

    padding-left: 40px;
}

.input-search{
    width:230px;
    height:20px;
    margin-left: 16px;

    border: none; 
    outline: none;

    &::placeholder{
    font-size: 14px;
    line-height: 18px;
    color:#C4C4C4;
}

}

.img-search{
    padding-left: 40px;
}

.sorting{
    display: flex;
    align-items: center;

    font-size: 14px;
    line-height: 18px;
}

.sorting-text{
    padding-right: 15px;
}

.sorting-inner{
    padding-right: 20px;
    
    border: none; 
    outline: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    background: url(../assets/img/caret.svg)  no-repeat right;
    background-position: right; 

    &:active {
            outline: none
    }

    &:focus {
            outline: none
    }

}

.todo-inner{
    padding-top: 30px;

    font-size: 14px;
    line-height: 18px;
}

.todo-header{
    display: flex;
    flex-direction: row;
    align-items: center;

    padding-bottom: 16px;

    max-width: 1300px;
}

.checkbox-col{
    min-width: 6.1%;
}

.description-col{
    min-width: 72%;  
    padding-left: 20px;
}

.status-col{
    min-width: 11.6%; 
    padding-left: 20px;
}

.date-col{
    min-width: 10.3%; 
    padding-left: 20px;
}

.line{
    border-left:1px solid #C4C4C4;
    height:32px;
}

</style>