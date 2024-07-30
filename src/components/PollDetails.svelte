<script>
    import PollStore from "../stores/PollStore";
    import Card from "../shared/Card.svelte";
    import Button from "../shared/Button.svelte";
    import { tweened } from "svelte/motion";
    import { cubicOut } from 'svelte/easing'; 

    export let poll;

    //reactive
    $: totalVotes = poll.votesA + poll.votesB;
    $: percentA = Math.floor((poll.votesA / totalVotes) * 100) || 0;
    $: percentB = Math.floor((poll.votesB / totalVotes) * 100) || 0;

    //tweened percentages
    const tweenedA = tweened(0, {
        duration: 200, // Duration in milliseconds
        easing: cubicOut // Optional easing function
    });
    const tweenedB = tweened(0, {
        duration: 200, // Duration in milliseconds
        easing: cubicOut // Optional easing function
    });

    $: tweenedA.set(percentA);
    $: tweenedB.set(percentB);
    $: console.log($tweenedA, $tweenedB);

    const handleVote = (option, id) => {
        PollStore.update((currentPolls) => {
            //this method does not subscribe to the PollStore, so we dont need to unsubscribe
            let copiedPolls = [...currentPolls];
            let modified = copiedPolls.find((poll) => poll.id === id);

            if (option === "a") {
                modified.votesA++;
            }
            if (option === "b") {
                modified.votesB++;
            }

            return copiedPolls; //the return updated the array, which now contains the poll with updated data
        });
    };

    //deleting a poll
    const handleDelete = (id) => {
        PollStore.update((currentPolls) => {
            return currentPolls.filter((poll) => poll.id != id); //includes all the items to the currentPolls that meet the condition
        });
    };
</script>

<Card>
    <div class="poll">
        <h3>{poll.question}</h3>
        <p>Total votes: {totalVotes}</p>

        <div class="answer" on:click={() => handleVote("a", poll.id)}>
            <div class="percent percent-a" style="width: {$tweenedA}%;"></div>
            <span>{poll.answerA} ({poll.votesA})</span>
        </div>

        <div class="answer" on:click={() => handleVote("b", poll.id)}>
            <div class="percent percent-b" style="width: {$tweenedB}%;"></div>
            <span>{poll.answerB} ({poll.votesB})</span>
        </div>
    </div>
    <div class="delete">
        <Button on:click={() => handleDelete(poll.id)}
            >Delete</Button
        >
    </div>
</Card>

<style>
    h3 {
        margin: 0 auto;
        color: grey;
    }

    p {
        margin-top: 6px;
        color: #ff3e00;
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
        background-color: rgba(0, 64, 255, 0.3);
    }

    .percent-b {
        background-color: rgba(255, 0, 0, 0.3);
    }

    .delete {
        margin-top: 20px;
        text-align: center;
    }
</style>
