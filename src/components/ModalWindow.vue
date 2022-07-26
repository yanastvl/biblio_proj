<template>
    <div class="fixed-overlay fixed-overlay__modal" id="modal" v-if="visible">
        <div class="modal" v-click-outside="hideModal">
            <div class="modal_container"> 
                <div class="modal-inner">
                    <div class="title-inner">
                        <h2 class="modal-caption">Создать новую задачу</h2>
                        <button @click="hideModal" class="modal-close">
                            <svg class="img-close" width="8" height="8" viewBox="0 0 8 8" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path d="M1 1L4 4M7 7L4 4M4 4L7 1M4 4L1 7" stroke="white" stroke-linecap="round"/>
                            </svg>
                        </button>
                    </div>
                    <form @submit="createTask">
                        <div class="description-inner">
                            <p class="modal-text">Описание</p>
                            <input class="modal-input" type="text" v-model="description" name="description" placeholder="Введите описание"/></div>
                        <div class="button-inner">
                        <button class="button-create" type="submit">Создать</button></div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
export default {
    data() {
        return {
            description: ''
        };
    },
    props: ["visible"],
    methods: {
        hideModal() {
            this.$emit('hideModal');
        },
        createUniqueKey() {
            return Array(1).fill(null).map(() => Math.random().toString(18).substr(2)).join('');
        },
        createTask(e) {
            e.preventDefault();
            const id = this.createUniqueKey();
            const task = {
                "id": id,
                "date": new Date(),
                "description": this.description,
                "status": "inProgress"
            }
            localStorage.setItem(id, JSON.stringify(task));
            this.$emit("update");
            this.hideModal();
            this.description = "";
        }
    },
};
</script>

<style lang="scss" scoped>
@import '../scss/vars.scss';

.fixed-overlay {
    position: fixed;
    overflow: auto;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgb(255 255 255 / 60%);
    backdrop-filter: blur(2px);
}

.fixed-overlay__modal {
    text-align: center;
    white-space: nowrap;
}

.fixed-overlay__modal::after {
    display: inline-block;
    vertical-align: middle;
    width: 0;
    height: 100%;
    content: '';
}

.modal {
    display: inline-block;
    vertical-align: middle;
    width:400px;
    height:281px;
}

.modal_container {
    padding: 40px 40px 50px 40px;
    text-align: left;
    white-space: normal;
    background-color: #fff;    
    color: #000;
    border: 1px solid #DDE2E4;
    box-shadow: 0px 25px 50px -12px rgb(0 0 0 / 25%);
    border-radius: 6px;
}

.modal-inner,.description-inner{
    display: flex;
    flex-direction: column;
}

.modal-close{
    width:22px;
    height:22px;
    background: #314B99;
    border-radius: 5px;
}

.title-inner{
    display: flex;
    justify-content: space-between;
}

.modal-caption{
    font-family: 'Montserrat', sans-serif;
    font-weight: 700;
    font-size: 18px;
    line-height: 24px;

    padding: 0px 62px 30px 0px;
}

.img-close{
    display: flex;
    margin-left: auto;
    margin-right: auto;
}

.modal-text{
    padding-bottom:5px;

    font-size: 14px;
    line-height: 14px;
}

.closed {
    display: none;
}

.modal-input{
    width:319px;
    height:40px;
    border: 1px solid #DDE2E4;
    border-radius: 8px;

    padding:11px 16px;

    font-size: 14px;
    line-height: 18px;

&::placeholder{
    opacity: 0.5;
}

}

.button-inner{
    display: flex;
    justify-content: center;
}

.button-create{
    margin-top: 30px;
    padding: 12px 40px;

    background: #F0F5FF;
    border-radius: 8px;

    color: $blue;
    font-size: 18px;
    line-height: 24px;
}

</style>