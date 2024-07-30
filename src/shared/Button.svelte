<script>
    export let type = "primary";
    export let flat = false;
    export let inverse = false;
</script>

<button class={type} class:flat={flat} class:inverse={inverse} on:click>
    <!-- on:click event forwarder is needed since this is a custom button element -->
    <slot></slot>
</button>

<style>
    button {
        border: 0;
        cursor: pointer;
        border-radius: 10px;
        padding: 8px 12px;
        font-weight: bold;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.3);
    }
    .primary {
        background-color: red;
        color: white;
    }

    .secondary {
        background-color: green;
        color: white;
    }

    .flat{
        box-shadow: none;
    }

    .primary.inverse{
        border: 3px solid red;
        background-color: white;
        color: red;
    }

    .secondary.inverse{
        border: 3px solid green;
        background-color: white;
        color: green;
    }
</style>
