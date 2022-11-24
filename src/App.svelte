<script>
	import DragAndDrop from "./DragAndDrop.svelte";
	import FileManager from "./FileManager.svelte";
	import FileListView from "./FileListView.svelte";

	let loadFiles;
	let files;
	// $: console.log(files);
</script>

	<DragAndDrop {loadFiles} />
	<FileManager bind:files={files} bind:loadFiles={loadFiles} />
	<FileListView {files} />
