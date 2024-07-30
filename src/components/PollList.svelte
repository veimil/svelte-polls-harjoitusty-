<script>
    import PollStore from "../stores/PollStore.js";
    import PollDetails from "./PollDetails.svelte";
    import { fade, slide, scale } from "svelte/transition"; //can be used with html elements, not custom components
    import { flip } from "svelte/animate";

    // const unsub = pollStore.subscribe(data => {  //get data from a store, returns a method which can be called to unsubscribe
    //     polls = data;
    // })

    // the callback function is triggered when the component is destroyed on the dom
    // onDestroy(() => {
    //     unsub(); //used alongside subscribe-method
    // })
</script>

<div class="poll-list">
    {#each $PollStore as poll (poll.id)}
        <!-- this way the store is automatically subscribed, and unsubscribed on destroy by Svelte -->
        <div in:fade out:scale|local animate:flip={{ duration: 200 }}>
            <!-- local applies the transition only to an individual item so it is applied only on delete  -->
            <PollDetails {poll} />
        </div>
    {:else}
        <div class="nopolls">
            <p>No polls to display</p>
        </div>
    {/each}
</div>

<style>
    .poll-list {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 20px;
    }
    .nopolls {
        height: 200px;
        text-align: center;
        margin: auto;
        padding: 30px 0;
        color: #ff3e00;
        font-size: 1.5em;
    }
</style>
