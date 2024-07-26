<script>
    import { createEventDispatcher } from "svelte";
    import Card from "../shared/Card.svelte";
    export let poll;

    const dispatch = createEventDispatcher();

    //reactive
    $: totalVotes = poll.votesA + poll.votesB;
    $: percentA = Math.floor(poll.votesA / totalVotes * 100);
    $: percentB = Math.floor(poll.votesB / totalVotes * 100);

    const handleVote = (option, id) => {
        dispatch("dispatchVote", { option, id });
    };
</script>

<Card>
    <div class="poll">
        <h3>{poll.question}</h3>
        <p>Total votes: {totalVotes}</p>

        <div class="answer" on:click={() => handleVote("a", poll.id)}>
            <div class="percent percent-a" style="width: {percentA}%;"></div>
            <span>{poll.answerA} ({poll.votesA})</span>
        </div>

        <div class="answer" on:click={() => handleVote("b", poll.id)}>
            <div class="percent percent-b" style="width: {percentB}%;"></div>
            <span>{poll.answerB} ({poll.votesB})</span>
        </div>
    </div>
</Card>

<style>
    h3 {
        margin: 0 auto;
        color: grey;
    }

    p {
        margin-top: 6px;
        color: blueviolet;
        margin-bottom: 30px;
    }

    .answer {
        background: #fafafa;
        cursor: pointer;
        margin: 10px auto;
        position: relative;
    }
    .answer:hover {
        opacity: 0.6;
    }

    span {
        display: inline-block;
        padding: 10px 20px;
    }

    .percent {
        position: absolute;
        height: 100%;
        box-sizing: border-box;
    }

    .percent-a {
        background-color: rgba(255, 0, 0, 0.2);
    }

    .percent-b {
        background-color: rgba(11, 158, 0, 0.2);
    }
</style>
