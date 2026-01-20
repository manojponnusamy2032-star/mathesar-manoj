<script lang="ts">
  import { faEye, faEyeSlash } from '@fortawesome/free-solid-svg-icons';
  import BaseInput from '@mathesar-component-library-dir/common/base-components/BaseInput.svelte';
  import Button from '@mathesar-component-library-dir/button/Button.svelte';
  import Icon from '@mathesar-component-library-dir/icon/Icon.svelte';

  import type { PasswordInputProps } from './PasswordInputTypes';

  type $$Props = PasswordInputProps;

  /**
   * Value of the input. Use bind tag for two-way binding.
   * Refer Svelte docs for more info on binding form input values.
   */
  export let value: $$Props['value'] = '';

  // Additional classes
  let classes = '';
  export { classes as class };

  // Underlying DOM element for direct access
  export let element: $$Props['element'] = undefined;

  export let hasError = false;

  // Id for the input
  export let id: $$Props['id'] = undefined;

  // Track visibility state
  let showPassword = false;

  // Handle toggle button click - must not submit form
  function togglePasswordVisibility(e: Event) {
    e.preventDefault();
    showPassword = !showPassword;
    // Focus the input after toggling
    element?.focus();
  }
</script>

<BaseInput {...$$restProps} bind:id />

<div class="password-input-wrapper">
  <input
    bind:this={element}
    {...$$restProps}
    type={showPassword ? 'text' : 'password'}
    class={['input-element', 'password-input', ...classes.split(' ')].join(' ')}
    class:has-error={hasError}
    bind:value
    {id}
    on:input
    on:focus
    on:blur
    on:keydown
    on:beforeinput
    on:change
  />
  <Button
    type="button"
    appearance="ghost"
    size="small"
    class="password-toggle-btn"
    aria-label={showPassword ? 'Hide password' : 'Show password'}
    aria-pressed={showPassword}
    on:click={togglePasswordVisibility}
    title={showPassword ? 'Hide password' : 'Show password'}
  >
    <Icon data={showPassword ? faEyeSlash : faEye} label={showPassword ? 'Hide password' : 'Show password'} />
  </Button>
</div>

<style>
  .password-input-wrapper {
    position: relative;
    display: flex;
    align-items: center;
  }

  :global(.password-input-wrapper .input-element) {
    padding-right: 2.5rem;
  }

  :global(.password-toggle-btn) {
    position: absolute;
    right: 0.25rem;
    margin: 0;
    padding: 0.25rem;
    background: transparent;
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--color-text-muted, #666);
    min-width: auto;
    min-height: auto;
  }

  :global(.password-toggle-btn:hover) {
    color: var(--color-text, #000);
  }

  :global(.password-toggle-btn:focus) {
    outline: 2px solid var(--color-primary, #4a90e2);
    outline-offset: 1px;
  }
</style>
