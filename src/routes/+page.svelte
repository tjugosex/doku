<script>
  import { Carta, MarkdownEditor } from "carta-md";
  import "carta-md/default.css";
  import { math } from '@cartamd/plugin-math';
  import { code } from '@cartamd/plugin-code';
  import { slash } from '@cartamd/plugin-slash';
  import "katex/dist/katex.css";
  import '@cartamd/plugin-code/default.css';
  let pdfUrl = "";
  const carta = new Carta({
    extensions: [math(), code(), slash()]
  });

  let value = "";

  function handleFileUpload(event) {
    const file = event.target.files[0];
    if (file && file.type === "application/pdf") {
      pdfUrl = URL.createObjectURL(file);
    }
  }
</script>

<div class="page">
  <input
    type="file"
    accept="application/pdf"
    on:change={handleFileUpload}
    class="file-input"
  />

  {#if pdfUrl}
    <div class="content">
      <div class="pdf">
        <iframe
          title="pdf"
          src={pdfUrl}
          width="100%"
          height="100%"
          style="border: none;"
        ></iframe>
      </div>
      <div class="editor">
        <MarkdownEditor bind:value {carta} />
      </div>
    </div>
  {:else}
    <div class="placeholder">
      <p>Select a PDF file to view.</p>
    </div>
  {/if}
</div>

<style>
  :global(html, body) {
    margin: 0;
    padding: 0;
    height: 100vh;
    overflow: hidden;
  }

  .page {
    height: 100vh;
    display: flex;
    flex-direction: column;
  }

  .file-input {
    flex-shrink: 0;
    padding: 4px;
  }

  .content {
    flex-grow: 1;
    display: flex;
    height: 100%;
  }

  .pdf,
  .editor {
    width: 50%;
    height: 100%;
  }

  .placeholder {
    flex-grow: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  :global(.carta-font-code) {
    font-family: "Courier New", monospace;
    font-size: 1.1rem;
    line-height: 1.1rem;
    letter-spacing: normal;
  }
</style>
