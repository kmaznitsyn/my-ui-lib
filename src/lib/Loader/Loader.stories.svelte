<script module>
  import { defineMeta } from '@storybook/addon-svelte-csf';
  import Loader from './Loader.svelte';

  const { Story } = defineMeta({
    title: 'UI/Loader',
    component: Loader,
    tags: ['autodocs'],
    argTypes: {
      size: {
        control: { type: 'select' },
        options: ['small', 'medium', 'large'],
      },
      color: { control: 'color' },
      label: { control: 'text' },
    },
  });
</script>

<Story name="Default" args={{}} />

<Story name="Small" args={{ size: 'small' }} />

<Story name="Medium" args={{ size: 'medium' }} />

<Story name="Large" args={{ size: 'large' }} />

<Story name="Custom Color" args={{ size: 'medium', color: '#6366f1' }} />
