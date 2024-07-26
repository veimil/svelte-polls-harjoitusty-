
<!-- reusable tab template -->
<script>
    import { createEventDispatcher } from "svelte";
    export let items;
    export let activeItem;

    const dispatch = createEventDispatcher();
</script>

<div class="tabs">
    <ul>
        {#each items as item}
            <li on:click={() => dispatch('tabChange', item)}>
                <div class:active={item === activeItem}>{item}</div>
            </li>
        {/each}
    </ul>
</div>

<style>
    .tabs {
        margin-bottom: 40px;
    }
    .tabs ul {
        display: flex;
        justify-content: center;
        padding: 0;
        list-style-type: none;
    }

    .tabs li {
        margin: 0 16px;
        font-size: 2em;
        color: #595959;
        cursor: pointer;
    }
    .active {
        color: red;
        border-bottom: 2px solid red;
        padding-bottom: 5px;
    }
</style>
