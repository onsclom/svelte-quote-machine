<script>
  import TwitterIcon from "@svelte-parts/icons/feather/twitter";
	import { fade } from 'svelte/transition';

  let curQuote = {
    content: "",
    author: "",
  };
	let loaded = false
	loadQuote()

  async function loadQuote() {
		loaded = false;
    let newQuote = await fetch("https://api.quotable.io/random");
    newQuote = await newQuote.json();
    curQuote = newQuote;
		loaded = true
  }
</script>

<main>
  <div id="quote-box">
		{#if loaded}
			<p id="text" transition:fade>
				{curQuote.content}
			</p>
			<p id="author" transition:fade>
				- <i>{curQuote.author}</i>
			</p>
		{/if}
    <div class="bottom-row">
      <button id="new-quote" on:click={loadQuote}>
				new quote
			</button>
      <a
        id="tweet-quote"
        href="https://twitter.com/intent/tweet?text={encodeURIComponent(
          curQuote.content + '\n\n' + '- ' + curQuote.author
        )}"
        target="_blank"
      >
        <TwitterIcon /> tweet
      </a>
    </div>
  </div>
</main>

<style>
  main {
    height: 100%;
    width: 100%;
    display: flex;
    background: var(--prussian-blue);
    font-size: 1.5rem;
  }
  #author {
    font-size: 1.4rem;
    text-align: center;
  }
  .bottom-row {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
  }
  #quote-box {
    max-width: 30rem;
    margin: auto;
    background: var(--papaya-whip);
    color: var(--prussian-blue);
    padding: 1.5rem;
    border-radius: 1rem;
  }
  a {
    display: block;
    font-size: 1.5rem;
    color: var(--prussian-blue);
  }
  button {
    background: var(--prussian-blue);
    color: var(--papaya-whip);
    padding: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;
    font-size: 1.4rem;
    border: 4px solid var(--air-superiority-blue);
  }
</style>
