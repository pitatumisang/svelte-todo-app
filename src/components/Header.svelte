<script>

    import Button from './Button.svelte'

</script>

<nav class="navbar util">
    <h1 class="title-1">todo</h1>
    <div>
        <p class="username">Tumisang</p>
        <Button btnType='secondary'>sign out</Button>
    </div>
</nav>

<style>
    .navbar{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .username{
        display: inline;
        color: var(--text-color);
    }

</style>