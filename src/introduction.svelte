<script>
import {db} from './firebase';
  export let uid;
  export let project;
  let sender_company_name = "Mukesh Joshi";
  let recipient_company_name = "OrangeWP";
  let exp_year = "10";
  let inputSizeArray = [];
  let team_member1 = "Mukesh Joshi/Developer";

function updateSenderCompanyName(){
  db.collection('users').doc(uid).collection('proposals').doc(project).update({
    senderName:sender_company_name
  });
}

function updateRecipientName(){
  db.collection('users').doc(uid).collection('proposals').doc(project).update({
    recipientName:recipient_company_name
  });
}

function updateExpYear(){
  db.collection('users').doc(uid).collection('proposals').doc(project).update({
    exp_year:exp_year
  });
}
  //   Re-size code
  $: inputSizeArray[0] = sender_company_name.length + 1;
  $: inputSizeArray[1] = recipient_company_name.length + 1;
  $: inputSizeArray[2] = exp_year.length + 1;
  $: inputSizeArray[3] = team_member1.length + 1;
</script>

<style>
  img {
    width: 100%;
  }
  input {
    border: none;
    border-bottom: 2px dotted #333;
  }

  @media print {
    input {
      font-weight: bold;
    }
  }
</style>
<div class="section">
  <h2>Introduction</h2>
  <p>
    <strong>
      <input
        size={inputSizeArray[0]}
        bind:value={sender_company_name}
        type="text" on:blur={updateSenderCompanyName} />
    </strong>
    <i>(hereafter known as the Provider)</i>
    is pleased to submit a proposal for the web site development of
    <strong>
      <input
        size={inputSizeArray[1]}
        bind:value={recipient_company_name}
        type="text" on:blur={updateRecipientName} />
    </strong>
    <i>(hereafter known as the Client)</i>
    . This proposal describes the technology, services, terms, and schedule for
    the web site development project.
  </p>
  <p>
    In the digital age, a web site frequently serves as your business or
    organization’s front entrance through which many customers will pass. Our
    mission is to give you a professional on-line presence that will generally
    enhance your organization’s image and branding, as well as move prospects
    through the sales process.
  </p>
  <img
    src="https://2fed6bb7db9480e47219-072828323aaa1fb7d24347f4f6835b12.ssl.cf5.rackcdn.com/proposals/ckeditor_image/image/proposal_attachment_96487f2c-56ea-46b6-8e74-a02355ed7594.jpeg"
    alt="Introduction Image"
    class="src" />
  <h3>About Us</h3>
  <p>
    {sender_company_name} is a top provider of professional web site design and
    development. We have over
    <input size={inputSizeArray[2]} bind:value={exp_year} on:blur={updateExpYear} type="text" />
    years of experience in the web development field with a long list of
    satisfied customers. We will be happy to provide additional references upon
    request. We have a clear understanding of the web site development
    requirements, and we have the knowledge, skills and experience to
    successfully complete the web site project.
  </p>
</div>
