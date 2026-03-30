<script lang="ts">
  interface Props {
    /** Size of the loader */
    size?: 'small' | 'medium' | 'large';
    /** Color of the spinner */
    color?: string;
    /** Accessible label */
    label?: string;
  }

  const { size = 'medium', color, label = 'Loading…' }: Props = $props();

  const sizeMap = { small: '1rem', medium: '2rem', large: '3rem' };
  const dimension = $derived(sizeMap[size]);
  const borderColor = $derived(color ?? 'currentColor');
</script>

<span
  role="status"
  aria-label={label}
  class={['loader', `loader--${size}`].join(' ')}
  style="--loader-size: {dimension}; --loader-color: {borderColor};"
></span>

<style>
  .loader {
    display: inline-block;
    width: var(--loader-size);
    height: var(--loader-size);
    border: calc(var(--loader-size) / 8) solid color-mix(in srgb, var(--loader-color) 25%, transparent);
    border-top-color: var(--loader-color);
    border-radius: 50%;
    animation: spin 0.75s linear infinite;
  }

  @keyframes spin {
    to { transform: rotate(360deg); }
  }
</style>
