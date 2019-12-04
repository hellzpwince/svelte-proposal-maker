<script>
  import { db } from "./firebase";
  export let uid;
  export let project;
  let projectDetailBuilderTitle = "";
  let projectDetailBuilderContent = "";
  let projectDetail = [
    {
      title: "Planning",
      content:
        "Here the scope, requirements, and description of the final web site will be determined and documented, including the overarching site goals, number of pages, site hierarchy, merchandizing needs, language and web tools used (i.e. Flash or carts), plus forms, animations/graphics, and other specialized content."
    },
    {
      title: "Concept Design",
      content:
        "Sketches and/or page mock-ups are created that reflect the general appearance and the look and feel of the website for visitors. Once these are reviewed and approved web development will begin."
    },
    {
      title: "Creation and Coding for Primary Pages",
      content:
        "Primary site (i.e. top level) web pages are created in the previously determined language (html, PHP, VBScipt) to meet all the appearance, performance, and content requirements. Then primary pages will be reviewed by the Client with appropriate feedback for revision."
    }
  ];
updateProjectDetails();
function updateProjectDetails(){
    db.collection("users")
    .doc(uid)
    .collection("proposals")
    .doc(project)
    .update({
      projectDetails: { ...projectDetail }
    });
}

  function projectDetailBuilder() {
    if (projectDetailBuilderTitle != "" && projectDetailBuilderContent != "") {
      projectDetail.push({
        title: projectDetailBuilderTitle,
        content: projectDetailBuilderContent
      });
      updateProjectDetails();
      projectDetail = projectDetail;
      projectDetailBuilderTitle = "";
      projectDetailBuilderContent = "";
    } else {
      alert("Please Enter Valid Content");
    }
  }

  function deleteItem(index) {
    var filtered = projectDetail.filter(function(value, i, arr) {
      return index != i;
    });
    projectDetail = filtered;
    updateProjectDetails();
  }
</script>

<style>
  .builder input {
    width: 100%;
    border: none;
    padding: 8px;
  }
  .project-item {
    margin-bottom: 20px;
  }
  .project-item button {
    font-size: 80%;
    color: white;
    border: none;
    background: red;
    display: block;
    float: right;
  }
</style>

<div class="section">
  <h2>Project Details</h2>
  <p>
    In order to develop a web site that fulfills all the goals of the web site
    development project, the proposed web development will take place in several
    distinct phases:
  </p>
  {#each projectDetail as detail, i}
    <div class="project-item">
      <h3>{i + 1}. {detail.title}</h3>
      <p>{detail.content}</p>
      <button on:click={() => deleteItem(i)}>Delete</button>
      <div class="clearfix" />
    </div>
  {/each}
  <div class="builder">
    <input
      type="text"
      bind:value={projectDetailBuilderTitle}
      placeholder="Enter Title" />
    <input
      type="text"
      bind:value={projectDetailBuilderContent}
      placeholder="Enter Content" />
    <button on:click={projectDetailBuilder}>Add</button>
  </div>
</div>
