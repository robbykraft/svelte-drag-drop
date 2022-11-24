<script>
	export let files = [];
	// files loaded after each drag event (supports multiple file upload)
	let batch = [];
	// hold onto each FileReader() so that after loading, we can
	// match the contents up with the file's metadata.
	let fileReaders = [];
	// counter will decrement after each load. 0 means all files are loaded.
	let asyncCount = 0;

	export let loadFiles = (event) => {
		if (event.dataTransfer.items) {
			asyncCount = event.dataTransfer.items.length;
			const transferFiles = [...event.dataTransfer.items]
				.filter(el => el.kind === "file")
				.map(el => el.getAsFile());
			batch = transferFiles.map(getMetadata);
			fileReaders = transferFiles.map(el => loadFile(el));
		}
		// else if (event.dataTransfer.files) {
		//  // Use DataTransfer interface to access the file(s)
		//  [...event.dataTransfer.files].forEach((file, i) => {
		//    console.log(`… 2.0 file[${i}].name = ${file.name}`);
		//  });
		// }
	};

	const getMetadata = (file) => ({
		type: file.type,
		name: file.name,
		size: file.size,
		lastModified: file.lastModified,
		contents: undefined,
	});

	const fileOnLoad = (event) => {
		const match = fileReaders
			.map((el, i) => (el === event.originalTarget ? i : undefined))
			.filter(el => el !== undefined)
			.shift();
		if (match === -1) {
			console.warn("load file interrupted");
			return;
		}
		batch[match].contents = event.target.result;
		asyncCount -= 1;
		// finished async loading all files
		if (asyncCount === 0) {
			files = files.concat(...batch);
		}
	};

	const readData = (file) => {
		const reader = new FileReader();
		reader.onload = fileOnLoad;
		reader.readAsArrayBuffer(file);
		return reader;
	};

	const readText = (file) => {
		const reader = new FileReader();
		reader.onload = fileOnLoad;
		reader.readAsText(file);
		return reader;
	};

	const loadFile = (file) => {
		// split the mime type into two parts and reference only the first
		switch (file.type.split("/")[0]) {
		case "image":
		case "audio":
		case "video":
		case "font":
			return readData(file);
		case "text":
			return readText(file);
		// need to consult specific mime types
		default:
			{
				switch (file.type) {
				case "application/json": return readText(file);
				case "application/pdf": return readData(file);
				default: return readData(file);
				}
			}
		}
	};
</script>
