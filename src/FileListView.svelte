<script>
	import FileItem from "./FileItem.svelte";
	import FileDescription from "./FileDescription.svelte";
	export let files = [];

	let selectedFile;
	let cursor = [0, 0];

	const onMouseMove = (e, file) => {
		selectedFile = file;
		cursor = [e.x - 10, e.y + 15];
	};
	const onMouseOut = () => selectedFile = undefined;

</script>

<div class="container">
	{#each files as file, i}
		<FileItem file={files[i]} {onMouseMove} {onMouseOut} />
	{/each}
</div>

{#if selectedFile !== undefined}
	<FileDescription file={selectedFile} {cursor} />
{/if}

<style>
	.container {
		width: 100%;
		padding: 1rem;
	}
</style>
