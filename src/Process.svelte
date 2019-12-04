<script>
  import { db } from "./firebase";
  import Icon from "fa-svelte";
  import { faTrashAlt } from "@fortawesome/free-solid-svg-icons/faTrashAlt";
  let projectDetailBuilderTitle = "";
  let projectDetailBuilderContent = "";
  let projectDetail = [
    "The Provider will assign a web project manager to oversee the project and serve as a single point of for project communication. The provider project team will also consist of additional web developers and other support staff members who will contribute as project requirements dictate.",
    "Project work will generally take place at the Provider facilities, however the Provider project manager will coordinate regular on-site visits on Client premises for meetings, evaluations, observations, reviews, testing, and other project purposes as needed.",
    "All project work will be in accordance with accepted best practices for web development.",
    "Key requirements for the web development project will be established and documented in Planning stage, and will be added as an Addendum to the Statement of Work."
  ];
  export let uid;
  export let project;
  let inputSizeArray = [];
  let processStartLine =
      "The web development project will be completed in the following way:",
    processEndLine =
      "This proposal only covers web site development. The Provider will be happy to provide an on-going maintenance and support proposal.";
  $: inputSizeArray[0] = processStartLine.length + 1;
  $: inputSizeArray[1] = processEndLine.length + 1;


updateProcess();
function updateProcess(){
    db.collection("users")
    .doc(uid)
    .collection("proposals")
    .doc(project)
    .update({
      Process: { ...projectDetail},
      processStart:processStartLine,
      processEnd:processEndLine
    });
}
  function projectDetailBuilder() {
    projectDetail.push("");
    projectDetail = projectDetail;
    updateProcess();
  }

  function deleteItem(index) {
    var filtered = projectDetail.filter(function(value, i, arr) {
      return index != i;
    });
    projectDetail = filtered;
    updateProcess();
  }
</script>

<style>
  .project-item input {
    border: 1px solid #ccc;
    padding: 8px;
    margin: 0px auto;
  }

  .fullwidth {
    margin-bottom: 30px;
  }
  input,
  textarea {
    border: none;
    width: 100%;
    border-bottom: 2px dotted #333;
  }

  .project-item {
    width: 100%;
    margin-bottom: 20px;
  }
  .project-item input {
    width: 90%;
  }
  .project-item button {
    max-width: 5%;
    padding: 12px auto;
    background: red;
    color: white;
    border: none;
  }
</style>

<div class="section">
  <h2>Our Process</h2>
  <div class="print">
    <p>{processStartLine}</p>
    <div>
      <ol>
        {#each projectDetail as detail, i}
          <li>{detail}</li>
        {/each}
      </ol>
    </div>
    <p>{processEndLine}</p>
  </div>
  <div class="screen">
    <textarea
      type="text"
      size={inputSizeArray[0]}
      bind:value={processStartLine} on:blur={updateProcess} />
    {#each projectDetail as detail, i}
      <div class="project-item">
        {i + 1}.
        <input
          type="text"
          placeholder="Describe Process Step here"
          bind:value={detail} />
        <button on:click={() => deleteItem(i)}>
          <Icon icon={faTrashAlt} />
        </button>
      </div>
    {/each}
    <div class="fullwidth">
      <button on:click={projectDetailBuilder}>+Add Item</button>
    </div>
    <textarea
      type="text"
      size={inputSizeArray[1]}
      bind:value={processEndLine} on:blur={updateProcess} />
  </div>
</div>
