<script lang="ts">
    import { onMount } from "svelte";
    import { onDestroy } from "svelte";

  let formHtml: HTMLFormElement;

  let valueA = "";
  let valueB = "";
  let value = "";

  let page = "A";
  const update = () => {
    if(page === "A") {
      value = valueA;
    } else {
      value = valueB;
    }
  }
  update();

  const onPageChange = (e:Event) => {
    e.preventDefault();
    // Autosave the value of the current page
    const formData = new FormData(formHtml);
    const currentVal = formData.get("val") as string;
    if(page === "A") {
      valueA = currentVal;
      page = "B";
    } else {
      valueB = currentVal;
      page = "A";
    }
    update();
  }
</script>

<main>
  <form bind:this={formHtml}>
  <div>Current Step : {page} -  ValueA: '{valueA}' -  ValueB: '{valueB}'  - Value : '{value}'</div>
  <div>
    Input Field (should be same as Value): <input name="val" value={value} />
  </div>
  <div>
    <button on:click={onPageChange}>Toggle Step</button>
  </div>
  </form>
</main>

<style>
  div {
    padding: 1.5em;
  }
</style>
