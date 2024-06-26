<script>
	$: questions = [];
	function getData() {
		const inputText = document.getElementById('inputText').value;
		const assignment = JSON.parse(inputText).medias[0];
		questions = generateQuestions(assignment.questions);
		console.log(questions);
		document.getElementById('answers').showModal();
	}

	function generateQuestions(data) {
    const questions = [];

    data.forEach(item => {
        // Calculate hours, minutes, and seconds
        const hours = Math.floor(item.time / 3600);
        const minutes = Math.floor((item.time % 3600) / 60);
        const seconds = Math.round(item.time % 60);

        const questionObj = {
            question: item.body[0].html.replace('<p>', '').replace('</p>', ''),
            correctAnswer: item.choices.find(choice => choice.isCorrect).body[0].html.replace('<p>', '').replace('</p>', ''),
            timeStamp: `${hours < 10 ? '0' + hours : hours}:${minutes < 10 ? '0' + minutes : minutes}:${seconds < 10 ? '0' + seconds : seconds}`
        };

        questions.push(questionObj);
    });

    return questions;
}

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />

</svelte:head>
<dialog id="answers" class="modal">
    <div class="modal-box p-4 rounded-lg shadow-lg max-w-lg">
        <div class="flex justify-between items-center mb-4">
            <h3 class="text-lg font-bold">Answers!</h3>
        </div>
        {#each questions as question}
        <div class="bg-gray-500 p-4 rounded-lg mb-4">
            <p class="font-bold text-white">{question.question} {question.timeStamp}</p>
            <p class="text-white">{question.correctAnswer}</p>
        </div>
        {/each}
        <p class="py-4 text-white">Press ESC key or click outside to close</p>
    </div>
    <form method="dialog" class="modal-backdrop"></form>
</dialog>

<main class="flex justify-center items-center h-screen">
	<div class="w-full max-w-lg bg-gray-700 rounded-lg shadow-lg p-6 text-center">
	  <h1 class="text-white py-4 text-3xl">EdPuzzle Answer generator</h1>
	  
	  <div class="relative" >
		<textarea id="inputText" class="textarea textarea-info text-white w-full max-w-lg" placeholder="Input Text to be corrected"></textarea>
	  </div>
	  
		<button
		  class="px-6 py-2 bg-blue-500 text-white mt-4 rounded-lg hover:bg-blue-600 focus:outline-none"
		  on:click={getData}
		>
		  Get Data
		</button>
	  
	</div>
  </main>

  
  