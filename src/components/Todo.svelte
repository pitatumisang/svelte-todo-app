<script>
    import {createEventDispatcher} from 'svelte';
    
    import Card from './Card.svelte';
    import Button from './Button.svelte'
    import Tag from './Tag.svelte'
    
    import TodoStore from '../stores/TodoStore.js'

    const dispatch = createEventDispatcher();

    export let todoList;

    const viewTodo = (id) => {
        dispatch('viewtodo',id);
    }

    const deleteTodo = (id) => {
        const result = todoList.filter(todo => id != todo.id);

        TodoStore.update(() => {return result});
    }

</script>

{#each todoList as todo }
    <Card>
        <div class="content">
            <p class="todo-title">{todo.title}</p>
            <div>
                <Button on:click = {() => {viewTodo(todo.id)}}>view</Button>
                <Button btnType='secondary' on:click={() => {deleteTodo(todo.id)}}>delete</Button>
            </div>
        </div>
        <Tag completed={todo.completed}/>
    </Card>
{/each}


<style>
    .content{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .todo-title{
        font-weight: 500;
        text-transform: uppercase;
        margin-bottom: 5px;
    }
</style>
