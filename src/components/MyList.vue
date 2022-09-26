<template>
        <!-- 1.原来的一层一层的传递 -->
        <!-- <ul class="todo-main">
            <MyItem v-for="todoObj in todos" 
            :key="todoObj.id" 
            :todo="todoObj" 
            :checkTodo="checkTodo"
            :deleteTodo="deleteTodo"
            />
        </ul> -->
        <ul class="todo-main">
            <transition-group name="todo" appear>
                <MyItem 
                    v-for="todoObj in todos"
                    :key="todoObj.id" 
                    :todo="todoObj" 
                />
		</transition-group>
        </ul>
</template>

<script>
    import MyItem from './MyItem.vue'

    export default {
        name:'MyList',
        components: {MyItem},
        // props:['todos','checkTodo','deleteTodo']
        props:['todos']

    }
</script>

<style scoped>
/*main*/
    .todo-main {
    margin-left: 0px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding: 0px;
    }

    .todo-empty {
    height: 40px;
    line-height: 40px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding-left: 5px;
    margin-top: 10px;
    }

    /* 下面为动画效果 */
    .todo-enter-active{
		animation: atguigu 0.5s linear;
	}

	.todo-leave-active{
		animation: atguigu 0.5s linear reverse;
	}
    @keyframes atguigu {
		from{
			transform: translateX(100%);
		}
		to{
			transform: translateX(0px);
		}
	}
</style>