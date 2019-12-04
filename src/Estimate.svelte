<script>
  import Icon from "fa-svelte";
  import { faTrashAlt } from "@fortawesome/free-solid-svg-icons/faTrashAlt";
  import { db } from "./firebase";
  export let uid, project;
  let estimateItem = "",
    estimateCost = 0,
    estimateUnit = "Flat Fee",
    estimateQty = 0,
    estimateSubtotal = 0;
  let estimateArray = [];
  let cartTotal = 0;

updateEstimate();
function updateEstimate(){
    db.collection("users")
    .doc(uid)
    .collection("proposals")
    .doc(project)
    .update({
      estimate: { ...estimateArray},
      cartTotal
    });
}
  function estimateAddItem() {
    if (
      estimateItem != "" &&
      estimateCost != "" &&
      estimateUnit != "" &&
      estimateQty != ""
    ) {
      estimateArray.push({
        item: estimateItem,
        cost: estimateCost,
        unit: estimateUnit,
        Qty: estimateQty
      });
      calculateCartTotal();
      estimateArray = estimateArray;
      updateEstimate();
      estimateUnit = "Flat Fee";
      estimateCost = 0;
      estimateItem = "";
      estimateQty = 0;
      estimateSubtotal = 0;
    } else {
      alert("Enter Valid Entries in Estimate Builder");
    }
  }

  function calculateCartTotal() {
    cartTotal = 0;
    estimateArray.map(item => {
      cartTotal += item.cost * item.Qty;
    });
  }

  function deleteItem(index) {
    var filtered = estimateArray.filter(function(value, i, arr) {
      return index != i;
    });
    estimateArray = filtered;
    calculateCartTotal();
    updateEstimate();
  }
</script>

<style>
  table {
    margin-top: 50px;
    width: 100%;
    text-align: left;
  }
  th,
  td {
    width: 16%;
  }
  .project-item input {
    border: 1px solid #ccc;
    padding: 8px;
    margin: 0px auto;
  }

  .fullwidth {
    margin-bottom: 30px;
  }
  .col-1 {
    width: 5%;
  }
  .col-2 {
    width: 10%;
  }
  input {
    border: none;
    width: 100%;
    border-bottom: 2px dotted #333;
  }

  .project-item {
    width: 100%;
    margin-bottom: 20px;
  }

  .fullwidth button {
    max-width: 100%;
  }
  tfoot td {
    font-weight: bold;
    padding: 5px;
    color: white;
    background: #999;
  }
</style>

<div class="section">
  <h2>Project Estimate</h2>
  <table>
    <thead>
      <tr>
        <th>Title</th>
        <th class="col-2">Cost</th>
        <th class="col-2">Unit</th>
        <th class="col-1">Qty</th>
        <th class="col-2">Subtotal</th>
        <th class="screen col-1">Action</th>
      </tr>
    </thead>
    <tbody>
      {#each estimateArray as item, i}
        <tr>
          <td>{item.item}</td>
          <td class="col-2">${item.cost}</td>
          <td class="col-2">{item.unit}</td>
          <td class="col-1">{item.Qty}</td>
          <td class="col-2">${item.cost * item.Qty}</td>
          <td class="screen col-1">
            <button
              style="background:red;color:white"
              on:click={() => deleteItem(i)}>
              <Icon icon={faTrashAlt} />
            </button>
          </td>
        </tr>
      {/each}
    </tbody>
    <tfoot>
      <tr>
        <td colspan="4" style="text-align:right">Total</td>
        <td colspan="2">${cartTotal}</td>
      </tr>
    </tfoot>
  </table>
  <div class="screen">
    <table class="project-item">
      <tbody>
        <tr>
          <td>
            <input
              type="text"
              bind:value={estimateItem}
              placeholder="Title"
              value="" />
          </td>
          <td class="col-2">
            <input
              type="number"
              bind:value={estimateCost}
              placeholder="Cost"
              value="" />
          </td>
          <td class="col-2">
            <select bind:value={estimateUnit}>
              <option value="Flat Fee">Flat Fee</option>
              <option value="Per Hour">Per Hour</option>
              <option value="Per Day">Per Day</option>
              <option value="Per Week">Per Week</option>
              <option value="Per Month">Per Month</option>
              <option value="Per Year">Per Year</option>
              <option value="Per Unit">Per Unit</option>
              <option value="Per Word">Per Word</option>
              <option value="Per Item">Per Item</option>
            </select>
          </td>
          <td class="col-1">
            <input
              type="number"
              bind:value={estimateQty}
              placeholder="Qty"
              value="" />
          </td>
        </tr>
      </tbody>
    </table>
    <div class="fullwidth">
      <button on:click={estimateAddItem}>+ Add Item</button>
    </div>
  </div>
</div>
