<script lang="ts">
  import type { TableEntry } from '@mathesar/api/tables/tableList';
  import { tables as tablesDataStore } from '@mathesar/stores/tables';
  import type { SelectProps } from '@mathesar-component-library/types';
  import SelectTable from './SelectTable.svelte';

  export let table: TableEntry | undefined = undefined;
  export let prependBlank = false;
  export let initialSelectionType: SelectProps<
    TableEntry | undefined
  >['initialSelectionType'] = 'first';

  $: tables = [...$tablesDataStore.data.values()];
</script>

<SelectTable
  {tables}
  bind:table
  {prependBlank}
  {initialSelectionType}
  on:change
/>
