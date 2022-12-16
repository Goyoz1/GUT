
<script>
	import { spring } from 'svelte/motion';

	/**
	 * @param {number} n
	 * @param {number} m
	 */
	function modulo(n, m) {
		// handle negative numbers
		return ((n % m) + m) % m;
	}

	/**
	 * @param {number} n
	 */
	async function getNextStep(n) {
		const res = await fetch(
			`https://83od0g6jn7.execute-api.eu-central-1.amazonaws.com/dev/GUT/GY?stepNumber=${n}`
		);
		const myStep = await res.json();

		if (res.ok) {
			return myStep;
		} else {
			throw new Error(JSON.stringify(myStep));
		}
	}

	var promise = getNextStep(0);

	let stepNumber = 0;
	$: promise = getNextStep(stepNumber);

	const displayed_count = spring();
	$: displayed_count.set(stepNumber);
	$: offset = modulo($displayed_count, 1);
</script>

<div class="GUT">
	<button on:click={() => (stepNumber -= 1)} aria-label="Decrease the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5" />
		</svg>
		Prev
	</button>

	<div class="counter-viewport">
		<div class="counter-digits" style="transform: translate(0, {100 * offset}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor($displayed_count + 1)}</strong>
			<strong>{Math.floor($displayed_count)}</strong>
		</div>
	</div>

	<button on:click={() => (stepNumber += 1)} aria-label="Increase the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
		</svg>
		Next
	</button>
</div>
<div class="row">
	{#await promise}
		<p>...</p>
	{:then step}
		<div class="column">
			ingredients
			<ul>
				{#each step.ingredients as ingredient}
					<li>{ingredient.name}</li>
				{/each}
			</ul>
		</div>
		<div class="column">
			container
			<ul>
				{#each step.container as myContainer}
					<li>{myContainer.name}</li>
				{/each}
			</ul>
		</div>
		<div class="column">
			operation
			<p>{step.operation}</p>
		</div>
		<div class="column">
			tools
			<ul>
				{#each step.tools as myTool}
					<li>{myTool.name}</li>
				{/each}
			</ul>
		</div>
		<div class="column">
			operation time
			<p>{step.operationTime}</p>
		</div>
	{:catch error}
		<p style="color: red">Not a valid step?</p>
	{/await}
</div>

<style>
	.row {
		display: flex;
	}

	.column {
		padding: 10px;
		height: 300px; /* Should be removed. Only for demonstration */
	}
	.GUT {
		display: flex;
		border-top: 1px solid rgba(0, 0, 0, 0.1);
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		margin: 1rem 0;
	}

	.GUT button {
		width: 2em;
		padding: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 0;
		background-color: transparent;
		touch-action: manipulation;
		font-size: 2rem;
	}

	.GUT button:hover {
		background-color: var(--color-bg-1);
	}

	svg {
		width: 25%;
		height: 25%;
	}

	path {
		vector-effect: non-scaling-stroke;
		stroke-width: 2px;
		stroke: #444;
	}

	.counter-viewport {
		width: 8em;
		height: 4em;
		overflow: hidden;
		text-align: center;
		position: relative;
	}

	.counter-viewport strong {
		position: absolute;
		display: flex;
		width: 100%;
		height: 100%;
		font-weight: 400;
		color: var(--color-theme-1);
		font-size: 4rem;
		align-items: center;
		justify-content: center;
	}

	.counter-digits {
		position: absolute;
		width: 100%;
		height: 100%;
	}

	.hidden {
		top: -100%;
		user-select: none;
	}
</style>
