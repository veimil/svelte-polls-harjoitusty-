<script>
    import PollStore from "../stores/PollStore";
    import { createEventDispatcher } from "svelte";
    import Button from "../shared/Button.svelte";
    let fields = { question: "", answerA: "", answerB: "" };
    let errors = { question: "", answerA: "", answerB: "" };
    let valid = false;

    let dispatch = createEventDispatcher();

    const submitHandler = () => {
        valid = true;

        //validate question
        if (fields.question.trim().length < 5) {
            valid = false;
            errors.question = "question must be at least 5 characters long";
        } else {
            errors.question = "";
        }

        //validate answer A
        if (fields.answerA.trim().length < 1) {
            valid = false;
            errors.answerA = "answer cannot be empty";
        } else {
            errors.answerA = "";
        }

        //validate answer B
        if (fields.answerB.trim().length < 1) {
            valid = false;
            errors.answerB = "answer cannot be empty";
        } else {
            errors.answerB = "";
        }
        //add the poll
        if (valid) {
            let poll = { ...fields, votesA: 0, votesB: 0, id: Math.random() };

            // save poll to the store
            PollStore.update((currentPolls) => {
                //the callback function takes the current data from the store
                return [poll, ...currentPolls]; //return updates the content of the store. No unsubscription needed with the update-method
            });
            dispatch("add"); //dispatching just the event identifier
        }
    };
</script>

<form on:submit|preventDefault={submitHandler}>
    <div class="form-field">
        <label for="question">Poll Question:</label>
        <input type="text" id="question" bind:value={fields.question} />
        <div class="error">{errors.question}</div>
    </div>
    <div class="form-field">
        <label for="answer-a">Answer A:</label>
        <input type="text" id="answer-a" bind:value={fields.answerA} />
        <div class="error">{errors.answerA}</div>
    </div>
    <div class="form-field">
        <label for="answer-b">Answer B:</label>
        <input type="text" id="answer-b" bind:value={fields.answerB} />
        <div class="error">{errors.answerB}</div>
    </div>
    <Button type="secondary">Add Poll</Button>
</form>

<style>
    form {
        width: 400px;
        margin: 0 auto;
        text-align: center;
    }
    .form-field {
        margin: 20px auto;
    }

    input {
        width: 100%;
        border-radius: 7px;
    }
    label {
        margin: 10px auto;
        text-align: left;
    }
    .error {
        color: red;
        text-align: left;
        font-size: 0.8em;
    }
</style>
