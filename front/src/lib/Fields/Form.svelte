<script lang="ts">
  import Input from "../../components/Input.svelte";
  import Button from "../../components/Button.svelte";
  import Select from "../../components/Select.svelte";
  import { INPUT_TYPES } from "../../constants/inputTypes";
  import { store } from "../../store.svelte";

  let key: string = $state("");
  let value: string = $state("");
  let type: string = $state("");
  let { route } = $props();
  const formId = route.result.querystring.original["form-id"];

  const submit = async (e: Event) => {
    store.isLoading = true;
    store.services
      .resource("forms")
      .update(formId, { key, value, type })
      .then(() => {
        key = "";
        value = "";
        type = "";
        store.services
          .resource(`forms`)
          .show(formId)
          .then((data: any) => {
            store.fields = data.form.fields;
            store.isLoading = false;
          });
      });
  };
</script>

<div class="mt-10 px-16 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-3">
  <div>
    <Input label="Key" name="key" type="text" bind:value={key} />
  </div>
  <div>
    <Input label="Value" name="value" type="text" bind:value />
  </div>
  <div>
    <Select label="Type" bind:value={type} options={INPUT_TYPES} />
  </div>
  <div class="col-span-3 flex items-center justify-end">
    <Button label="Guardar" onClick={submit} />
  </div>
</div>
