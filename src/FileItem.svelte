<script>
	import FileIcon from "./FileIcon.svelte";
	export let file;
	export let onMouseMove = () => {};
	export let onMouseOut = () => {};
</script>

<div class="item"
	on:mousemove={(e) => onMouseMove(e, file)}
	on:mouseout={() => onMouseOut()}>
	<FileIcon {file} /><p>{file.name}</p>
</div>

<style>
	.item {
		display: flex;
		flex-direction: row;
		justify-content: start;
		align-items: center;
		margin: 0.5rem 0;
	}
	p { flex: 1 1; }
	.item:first-child { margin-top: 0; }
	.item p { margin: 0 0 0 0.5rem; }
	.item:hover { cursor: pointer; }
	.item:hover p { color: #fb4; }
	.item :global(svg) { stroke: #eee; }
	.item:hover :global(svg) { stroke: #fb4; }
</style>