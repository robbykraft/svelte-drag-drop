<script>
	export let cursor = [0, 0];
	export let file;
</script>

<div class="hoverbox" style="left: {cursor[0]}px; top: {cursor[1]}px;">
	<p>size: <span>{file.size} bytes</span></p>
	<p>type: <span>{file.type}</span></p>
	{#if typeof file.contents === "string"}
		<p>contents:</p><pre>{file.contents.substring(0, 60)}{file.contents.length > 60 ? "..." : ""}</pre>
	{/if}
</div>

<style>
	.hoverbox {
		pointer-events: none;
		position: absolute;
		background-color: #181818;
		padding: 0.5rem;
	}
	pre {
		margin: 0;
		padding: 0;
		font-size: 1rem;
		font-weight: bold;
	}
	p {
		margin: 0;
		color: #aaa;
	}
	span {
		color: white;
		font-weight: bold;
	}
</style>
