<script>
    export let type = "primary";
    export let inverse = false;
</script>

<button class={type} class:inverse on:click>
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
    }
    .primary {
        background-color: red;
        color: white;
    }

    .secondary {
        background-color: green;
        color: white;
    }

    .primary.inverse {
        border: 3px solid red;
        background-color: white;
        color: red;
    }

    .secondary.inverse {
        border: 3px solid green;
        background-color: white;
        color: green;
    }
</style>
