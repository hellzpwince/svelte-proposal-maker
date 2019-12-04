<script>
  import Details from "./Details.svelte";
  import Introduction from "./Introduction.svelte";
  import WhatWeDo from "./WhatWeDo.svelte";
  import Process from "./Process.svelte";
  import Deliverables from "./Deliverables.svelte";
  import Estimate from "./estimate.svelte";
  import html2canvas from "html2canvas";
  import jsPDF from 'jsPDF';
  import { auth, googleProvider, db } from "./firebase";
  import { authState } from "rxfire/auth";
  let user = authState(auth);
  function login() {
    auth.signInWithPopup(googleProvider);
  }
  let documentId = "null";
  let userId = "";
  $: if ($user) {
    userId = $user.uid;
    db.collection("users")
      .doc($user.uid)
      .collection("proposals")
      .add({
        senderName: "Mukesh Joshi"
      })
      .then(function(doc) {
        documentId = doc.id;
      });
  }
function printPdf(){
  window.print();
}
</script>

<style>
  .main {
    max-width: 800px;
    margin: auto;
    padding: 20px;
  }
  :global(.section) {
    margin-top: 40px;
  }
  :global(.clearfix) {
    display: block;
    content: "";
    clear: both;
  }
  :global(textarea) {
    line-height: 1.6;
  }
  :global(h2) {
    font-size: 29pt;
    color: #808080;
    font-weight: normal;
  }
  :global(p, li) {
    color: #222;
    line-height: 1.6;
  }

  :global(.builder) {
    padding: 20px 10px;
    border-radius: 5px;
    background: rgb(240, 239, 239);
  }
  :global(.fullwidth button) {
    width: 100%;
  }
  :global(button) {
    margin-top: 10px;
    background: #3498db;
    color: white;
    padding: 10px;
    border: none;
    cursor: pointer;
  }
  :global(.print) {
    display: none;
  }

  :global(ol li) {
    margin-bottom: 10px;
  }
  /* Print CSS */
  @page {
    size: auto;
    margin: 15mm;
  }
  @media print {
    :global(button, a) {
      display: none !important;
    }
    :global(.screen) {
      display: none;
    }
    :global(.print) {
      display: block;
    }
    :global(.section) {
      width: 100%;
      page-break-before: always;
    }
    :global(input, textarea) {
      border: none !important;
      padding: 0;
      resize: none;
      margin: 0;
    }
    :global(.project-item) {
      padding: 0 !important;
      margin: 0 !important;
    }
    :global(h2) {
      color: #ccc !important;
      margin-top: 5px;
      margin-bottom: 30px;
    }
    :global(.builder) {
      display: none;
    }
    :global(p, li, textarea) {
      line-height: 1.5 !important;
    }
  }
</style>

<section>
  {#if $user && documentId != 'null'}
    <div class="main">
      <Introduction uid={userId} project={documentId} />
      <Details uid={userId} project={documentId} />
      <WhatWeDo uid={userId} project={documentId} />
      <Process uid={userId} project={documentId} />
      <Deliverables uid={userId} project={documentId} />
      <Estimate uid={userId} project={documentId} />
    </div>
    <button on:click={() => auth.signOut()}>Logout</button>
    <button on:click={printPdf}>Download PDF</button>
  {:else}
    <button on:click={login}>Signin with Google</button>
  {/if}
</section>
