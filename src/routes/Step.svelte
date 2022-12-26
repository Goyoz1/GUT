<script>
export let theInternalStep
</script>
{#if !theInternalStep }
{:else}
<div>
	<p><textarea bind:value={theInternalStep.operation}></textarea></p>
</div>

		<div class="column">
			ingredients
			<ul>
				{#each theInternalStep.ingredients as ingredient}
					<li>{ingredient.name}</li>
				{/each}
			</ul>
		</div>
		<div class="column">
			container
			<ul>
				{#each theInternalStep.container as myContainer}
					<li>{myContainer.name}</li>
				{/each}
			</ul>
		</div>
		<div class="column">
			operation
			<p>{theInternalStep.operation}</p>
		</div>
		<div class="column">
			tools
			<ul>
				{#each theInternalStep.tools as myTool}
					<li>{myTool.name}</li>
				{/each}
			</ul>
		</div>
		<div class="column">
			operation time
			<p>{theInternalStep.operationTime}</p>
		</div>


<p>{JSON.stringify(theInternalStep)}</p>
{/if}

