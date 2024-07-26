<script>
	import Header from "./components/Header.svelte";
	import Footer from "./components/Footer.svelte";
	import Tabs from "./shared/Tabs.svelte";
	import CreatePollForm from "./components/CreatePollForm.svelte";
	import PollList from "./components/PollList.svelte";

	let items = ["Current Polls", "Add New Poll"];
	let activeItem = "Current Polls";

	const tabChange = (e) => {
		activeItem = e.detail;
	};

	// polls
	let polls = [
    {
      id: 1,
      question: 'Python or JavaScript?',
      answerA: 'Python',
      answerB: 'JavaScript',
      votesA: 9,
      votesB: 15,
    },
  ];

  const handleAdd = (e) => {
	const poll = e.detail;
	polls = [poll, ...polls];
	activeItem = 'Current Polls';
  }	

  const vote = (e) => {
	const {option, id} = e.detail;
	let copiedPolls = [...polls];
	let modified = copiedPolls.find((poll) => poll.id === id);

	if (option === 'a') {
		modified.votesA++;
	}
	if (option === 'b') {
		modified.votesB++;
	}

	polls = copiedPolls;
  }

</script>

<main>
	<Header />
	<Tabs {activeItem} {items} on:tabChange={tabChange} />
	<p>
		
	</p>
	{#if activeItem === 'Current Polls'}
	<PollList {polls} on:dispatchVote={vote}/>
	{:else if activeItem === "Add New Poll"}
	<CreatePollForm on:add={handleAdd} />
	{/if}

	<Footer />
</main>

<style>
	main {
		max-width: 960px;
		margin: 40px auto;
	}
</style>
