<script lang="ts">
    import { onMount } from "svelte";
    import { onDestroy } from "svelte";

  let formHtml: HTMLFormElement;

  let fieldValue = "";
  let valueA = "";
  let valueB = "";
  let useA = true;

  const update = () => {
    fieldValue = useA ? valueA : valueB;
  }
  update();

  const accept = (e:Event) => {
    e.preventDefault();
    // Autosave the value of the current page
    const formData = new FormData(formHtml);
    const val = formData.get("val") as string;
    if(useA) {
      valueA = val;
      useA = false;
    } else {
      valueB = val;
      useA = true;
    }
    update();
  }
</script>

<main>
  <form bind:this={formHtml}>
    <div>Using the A Value : {useA}</div>
    <div>ValueA: '{valueA}'</div>
    <div>ValueB: '{valueB}'</div>
    <div>Field Value (should be reflected in the Input field): '{fieldValue}'</div>

    <div>
      Input Field (<b>should be same as Field Value</b>): <input name="val" value={fieldValue} />
    </div>

    <div>
      <button on:click={accept}>Store Input value in {useA ? 'valueA' : 'valueB'}</button>
    </div>
  </form>
  <div class="explain">
    <u>Bug reproduction</u>: 
    <br/>
    Reload the page. Enter 'aaa' in the input field and click on "Store Input value in valueA".
    <br/>
    Observe that the value in "Field Value" is (correctly) ""
    <br/>
    But that the value in the input field is still (incorrectly) "aaa"
    <br/>
    <b>(the &lt;input value=&#123;fieldValue&#125;&gt; field value has not been updated by the assignemnt of fieldValue variable)</b>
  </div>
</main>

<style>
  div {
    padding: 5px;
    text-align: left;
  }
  .explain {
    font-family: 'Courier New', Courier, monospace;
    font-size: 80%;
  }
</style>
