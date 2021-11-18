<script>
	import Header from "./components/Header.svelte";
	import Sidebar from "./components/Sidebar.svelte";
	import Modal from "./components/Modal.svelte";
	import Todo from "./components/Todo.svelte";
	import FloatingButton from './components/FloatingButton.svelte';
	import AddTodoForm from "./components/AddTodoForm.svelte";
	import UpdateTodoForm from './components/UpdateTodoForm.svelte';
	import Alert from './components/Alert.svelte'

	import TodoStore from './stores/TodoStore.js'

	let isModalActive = false;
	let modalType;

	let singleTodo;
	let todoList;
	
	$: todoList= $TodoStore;

	let contentTitle = 'all todo items';

	const toggleModal = (type) => {

		modalType = type;
		isModalActive = !isModalActive
	}

	const viewTodoDetails = (e) => {
		toggleModal('viewtodo');
		const id = e.detail;
		singleTodo = todoList.find(todo => todo.id === id);
	}

	const viewCompletedTodo = () => {
		contentTitle = 'completed todo items'
		const completedTodo = $TodoStore.filter(todo => todo.completed === true)

		todoList = completedTodo;

	}

	const viewAllTodo = () => {
		contentTitle = 'all todo items';
		todoList = $TodoStore;
	}
	 
</script>

<main>
	{#if isModalActive}
		{#if modalType === 'addtodo'}
			<Modal on:click={toggleModal}>
				<AddTodoForm />
			</Modal>
		{:else if modalType === 'viewtodo'}
			<Modal on:click={toggleModal}>
				<UpdateTodoForm {...singleTodo}/>
			</Modal>
		{/if}
	{/if}

	<Header/>
	<section class="content-box">
		<Sidebar on:viewcompletedtodo={viewCompletedTodo} on:viewalltodo={viewAllTodo} />
		<div class="todo-content util">
			{#if todoList.length === 0}
				<h2>There are no todo items found!</h2>
			{:else}	
				<p class="title-2">{contentTitle}</p>		
				<Todo on:viewtodo={viewTodoDetails} {todoList}/>		
			{/if}
		</div>
	<FloatingButton on:click={() => {toggleModal('addtodo')}}>
			<h1>+</h1>
		</FloatingButton>
	</section>
</main>

<style>

	.content-box{
		display: grid;
		grid-template-areas: 'sidebar content content content';
		gap: 10px;
		margin-top: 10px;
		height: calc(100vh - 101px );
	}	

	.todo-content{
		grid-area: content;
		height: 100%;
        overflow: hidden;
	}

	.todo-content h2{
		text-align: center;
		margin-top: 20%;
	}
    	
</style>