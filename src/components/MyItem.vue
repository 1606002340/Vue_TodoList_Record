<template>
    <div>   
        <li>
            <label>
            <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
            <span v-show="!todo.isEdit">{{todo.title}}</span>
            <input v-show="todo.isEdit" type="text" 
                    :value="todo.title" 
                    @blur="handleBlur(todo,$event)" 
                    ref="inputTitle"
                    >
            </label>
            <button class="btn btn-danger" @click="HandleDelete(todo.id)">删除</button>
            <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo)">编辑</button>
        </li>
    </div>
</template>

<script>
    import pubsub from 'pubsub-js'
export default {
    name:'MyItem',
    // 声明接受插值对象
    //1.原来的一层一层的传递 -->
    // props:["todo","checkTodo","deleteTodo"],
    props:["todo"],
    methods:{
        // 勾选或者取消勾选
        handleCheck(id){
            // console.log(id);
            // 通知App将done的数值取反
            //1.原来的一层一层的传递 -->
            // this.checkTodo(id);
            this.$bus.$emit('checkTodo',id)

        },
        // 删除
        HandleDelete(id){
            // console.log(id);
            if(confirm("确定要删除吗？")){
                // this.deleteTodo(id)
                // this.$bus.$emit('deleteTodo',id)
                pubsub.publish('deleteTodo',id)
            }
        }, 
        // 编辑
        handleEdit(todo){
            if(todo.hasOwnProperty.call('isEdit')){
                todo.isEdit = true
            }
            else{
                this.$set(todo,'isEdit',true)
            }
            this.$nextTick(function(){
					this.$refs.inputTitle.focus()
				})
        },
        //失去焦点回调（真正执行修改逻辑）
        handleBlur(todo,e){
            todo.isEdit=false
            if(!e.target.value.trim()) return alert('输入不能为空！')
				this.$bus.$emit('updateTodo',todo.id,e.target.value)
        }
    }
}
</script>

<style scoped>
/*item*/
    li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
    }

    li label {
    float: left;
    cursor: pointer;
    }

    li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
    }

    li button {
    float: right;
    display: none;
    margin-top: 3px;
    }

    li:before {
    content: initial;
    }

    li:last-child {
    border-bottom: none;
    }
    li:hover{
        background-color: #ddd;
    }
    li:hover button{
        display: block;
    }
</style>