<script lang="ts">
  import { AttachableDropdown } from '@mathesar/component-library';
  import NameWithIcon from '@mathesar/components/NameWithIcon.svelte';
  import BreadcrumbSeparatorIcon from './BreadcrumbSeparatorIcon.svelte';
  import type { BreadcrumbSelectorData } from './breadcrumbTypes';

  export let data: BreadcrumbSelectorData;
  export let triggerLabel: string;

  let triggerElement: HTMLButtonElement;
  let isOpen = false;
</script>

<div class="entity-switcher" class:is-open={isOpen}>
  <button
    class="trigger passthrough-button"
    bind:this={triggerElement}
    on:click={() => {
      isOpen = !isOpen;
    }}
    aria-label={triggerLabel}
    title={triggerLabel}
  >
    <BreadcrumbSeparatorIcon />
  </button>

  <AttachableDropdown
    {isOpen}
    trigger={triggerElement}
    closeOnInnerClick
    on:close={() => {
      isOpen = false;
    }}
  >
    <div class="entity-switcher-content">
      {#each [...data] as [categoryName, items] (categoryName)}
        <div class="category-name">{categoryName}</div>
        <ul class="items">
          {#each items as { href, label, icon } (href)}
            <li class="item">
              <a {href}>
                <NameWithIcon {icon}>{label}</NameWithIcon>
              </a>
            </li>
          {/each}
        </ul>
      {/each}
    </div>
  </AttachableDropdown>
</div>

<style>
  .entity-switcher-content {
    padding: 0.5rem;
    min-width: 12rem;
  }
  .category-name {
    font-size: var(--text-size-x-small);
    margin: 0.25rem 0;

    color: var(--color-text-muted);
    text-transform: uppercase;
  }
  .items {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  .item a {
    display: block;
    padding: 0.25rem;
    border-radius: 4px;
    color: inherit;
    text-decoration: none;
  }
  .item a:hover {
    background: var(--color-contrast-light);
  }
  /* TODO: reduce code duplication with this CSS used elsewhere. */
  .passthrough-button {
    background: inherit;
    border-radius: inherit;
    border: inherit;
    color: inherit;
    cursor: inherit;
    font-family: inherit;
    font-size: inherit;
    font-weight: inherit;
    text-align: inherit;
    margin: 0;
    padding: 0;
  }
  .entity-switcher .trigger {
    --background-color: var(--color-gray-lighter);
    --border-color: var(--color-gray-dark);
    display: block;
    cursor: pointer;
  }
  .entity-switcher .trigger :global(svg) {
    height: 1.8rem;
    display: block;
  }
  .entity-switcher .trigger:hover,
  .entity-switcher.is-open .trigger {
    --background-color: var(--color-contrast);
    --icon-color: white;
    --border-color: var(--color-contrast);
  }
</style>
