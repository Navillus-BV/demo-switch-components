<script lang="ts">
  import { createEventDispatcher, getContext } from "svelte";
  import { uuid } from "../utils/id";

  export let label: string;
  export let checked = false;
  export let value = "on";
  export let required: boolean | undefined = undefined;
  export let disabled = false;
  export let id: string | undefined = uuid();

  let className = "svelte-switch";
  export { className as class };

  const dispatch = createEventDispatcher<{ change: boolean; click: Event }>();

  function onClick(event: Event) {
    checked = !checked;
    dispatch("click", event);
  }

  $: dispatch("change", checked);
</script>

<div class={className}>
  <label for={id}>{label}</label>

  <button
    type="button"
    role="switch"
    {id}
    aria-checked={checked}
    on:click={onClick}
    {disabled}
    {value}
  >
    <span />
  </button>

  <input
    class="sr-only"
    type="checkbox"
    aria-hidden
    tabindex="-1"
    bind:checked
  />
</div>

<style>
  div {
    display: flex;
    align-items: center;
    margin: 1em 0;
  }

  label {
    margin: 0;
    padding-right: 1em;
  }

  button {
    all: unset;
    width: 42px;
    height: 25px;
    background-color: rgba(0, 0, 0, 0.44);
    border-radius: 9999px;
    position: relative;
    box-shadow: 0 2px 4px #303030;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  }

  span {
    display: block;
    width: 21px;
    height: 21px;
    background-color: white;
    border-radius: 9999px;
    box-shadow: 0 2px 2px #303030;
    transition: transform 100ms;
    transform: translateX(2px);
    will-change: transform;
  }

  button:focus {
    box-shadow: 0 0 0 2px black;
  }

  button[aria-checked="true"] {
    background-color: black;
  }

  button[aria-checked="true"] span {
    transform: translateX(19px);
  }
</style>
