<script>
  import Icon from "fa-svelte";
  import { faTrashAlt } from "@fortawesome/free-solid-svg-icons/faTrashAlt";
  import { db } from "./firebase";
  export let uid, project;
  let projectDetailBuilderTitle = "",
    projectDetailBuilderContent = "",
    milestoneitem = "",
    milestoneDate = "";
  let projectDetail = [
    "Fully functional and approved web site in the required language(s) uploaded to the clients designated servers.",
    "A Wordpress feature allowing the Client to easily add blog postings."
  ];
  let deliverableStartLine =
      "The Provider will deliver the following items during or at the conclusion of the software development project:",
    deliverableEndLine =
      "The timeline for project milestones and deliverables is listed in Table 1.";

  let inputSizeArray = [];
  $: inputSizeArray[0] = deliverableStartLine.length + 1;
  $: inputSizeArray[1] = deliverableEndLine.length + 1;

  let milestonesArray = [];

updateDeliveries();
function updateDeliveries(){
    db.collection("users")
    .doc(uid)
    .collection("proposals")
    .doc(project)
    .update({
      Deliverable: { ...projectDetail},
      milestones:{...milestonesArray},
      deliveryStart:deliverableStartLine,
      deliveryEnd:deliverableEndLine
    });
}
  function projectDetailBuilder() {
    updateDeliveries();
    projectDetail.push("");
    projectDetail = projectDetail;
  }

  function milestoneAddItem() {
    if (milestoneitem != "" && milestoneDate != "") {
      milestonesArray.push({
        item: milestoneitem,
        date: milestoneDate
      });
      milestoneitem = "";
      milestoneDate = "";
      milestonesArray = milestonesArray;
      updateDeliveries();
    }
  }

  function deleteItem(index) {
    var filtered = projectDetail.filter(function(value, i, arr) {
      return index != i;
    });
    projectDetail = filtered;
    updateDeliveries();
  }

   function deleteMilestoneItem(index) {
    var filtered = milestonesArray.filter(function(value, i, arr) {
      return index != i;
    });
    milestonesArray = filtered;
  }
</script>

<style>
  table {
    margin-top: 50px;
    width: 100%;
  }
  table tr {
    text-align: left;
  }
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
  .col-8 {
    width: 80%;
  }
  .col-4 {
    width: 40%;
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
  .fullwidth button {
    max-width: 100%;
  }
</style>

<div class="section">
  <h2>Deliverables</h2>
  <div class="print">
    <p>{deliverableStartLine}</p>
    <div>
      <ol>
        {#each projectDetail as detail, i}
          <li>{detail}</li>
        {/each}
      </ol>
    </div>
    <p>{deliverableEndLine}</p>
  </div>
  <div class="screen">
    <textarea
      type="text"
      size={inputSizeArray[0]}
      bind:value={deliverableStartLine} on:blur={updateDeliveries} />
    {#each projectDetail as detail, i}
      <div class="project-item">
        {i + 1}.
        <input
          type="text"
          placeholder="Describe deliverable Step here"
          bind:value={detail} />
        <button on:click={() => deleteItem(i)}>
          <Icon icon={faTrashAlt} />
        </button>
      </div>
    {/each}
    <div class="fullwidth">
      <button on:click={projectDetailBuilder}>+Add Deliverable</button>
    </div>
    <textarea
      type="text"
      size={inputSizeArray[1]}
      bind:value={deliverableEndLine} on:blur={updateDeliveries} />
  </div>
  <table>
    <thead>
      <tr>
        <th>Milestones</th>
        <th>Description</th>
        <th>Complete</th>
        <th class="screen">Action</th>
      </tr>
    </thead>
    <tbody>
      {#each milestonesArray as milestone, i}
        <tr>
          <td>{i + 1}.</td>
          <td>{milestone.item}</td>
          <td>{milestone.date}</td>
          <td class="screen">
            <button
              style="background:red;color:white"
              on:click={() => deleteMilestoneItem(i)}>
              <Icon icon={faTrashAlt} />
            </button>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
  <div class="screen">
    <table class="project-item">
      <tbody>
        <tr>
          <td class="col-8">
            <input
              type="text"
              bind:value={milestoneitem}
              placeholder="Describe Milestone here"
              value="" />
          </td>
          <td class="col-4">
            <input
              type="date"
              placeholder="Completion Date"
              bind:value={milestoneDate} />
          </td>
        </tr>
      </tbody>
    </table>
    <div class="fullwidth">
      <button on:click={milestoneAddItem}>+Add Milestone</button>
    </div>
  </div>
</div>
