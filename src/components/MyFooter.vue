<template>
    <div>
        <div class="todo-footer" v-show="total">
            <label>
                <!-- 1. -->
                <!-- <input type="checkbox" :checked="isAll" @click="checkAll"/> -->
                <!-- 2. -->
                <input type="checkbox" v-model="isAll"/>
            </label>
            <span>
                <span>已完成{{doneTotal}}</span> / 全部{{total}}
            </span>
            <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
        </div>
    </div>
</template>

<script>
export default {
    name:'MyFooter',
    // props:['todos','checkAllTodo','clearAllTodo'],
    props:['todos'],
    computed:{
        total(){
            return this.todos.length
        },
        doneTotal(){
            // let i=0
            // this.todos.forEach((todo)=>{
            //     if(todo.done) i++
            // })
            // return i
            const x = this.todos.reduce((pre,current)=>{
                // console.log("@",pre,current);
                return pre + (current.done ? 1 : 0)
            },0)
            //console.log(x);
            return x
        },
        //1.
        // isAll(){
        //     return this.doneTotal == this .total && this.total > 0
        // }
        // 2.
        isAll:{
            get(){
                return this.doneTotal == this .total && this.total > 0
            },
            set(value){
                // this.checkAllTodo(value)
                this.$emit('checkAllTodo',value)//这里是用的自定义事件
            }
        }
    },
    methods:{
        // checkAll(e){
        //     // console.log(e.target.checked);
        //     this.checkAllTodo(e.target.checked)
        // }
        clearAll(){
            // this.clearAllTodo()
            this.$emit('clearAllTodo')//这是用的自定义事件
        }
    }

}
</script>

<style scoped>
/*footer*/
    .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
    }

    .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
    }

    .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
    }

    .todo-footer button {
    float: right;
    margin-top: 5px;
    }
</style>