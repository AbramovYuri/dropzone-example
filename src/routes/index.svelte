<script context="module">
    export const ssr = false;  //@TODO Dropzone not worked if ssr enabled   https://github.com/thecodejack/svelte-file-dropzone/issues/81
</script>

<script>
    import Dropzone from "svelte-file-dropzone";

    let files = {
        accepted: [],
        rejected: []
    };

    function handleFilesSelect(e) {
        const {acceptedFiles, fileRejections} = e.detail;
        files.accepted = [...files.accepted, ...acceptedFiles];
        files.rejected = [...files.rejected, ...fileRejections];
    }

    function handleRemoveAll() {
        files.accepted = [];
    }
</script>
<Dropzone
        on:drop={handleFilesSelect}
        accept={['application/pdf']}
        multiple={false}
        containerClasses="custom-dropzone">

    <div class="pdf-icon-wrapper">
        <button class="remove-file" on:click={handleRemoveAll} class:active="{files.accepted.length > 0}">X</button>
    </div>
    {#if files.accepted.length === 0}
        <p class="information">Drag and drop signed PDF or tap to upload</p>
    {:else }
        {#each files.accepted as item, index}
            <p class="information">{item.name}</p>
        {/each}
    {/if}
    <p class="footnote">Only single PDF file allowed</p>
</Dropzone>
