<script module>
  import { defineMeta } from '@storybook/addon-svelte-csf';
  import UVIndexChart from './UVIndexChart.svelte';

  const { Story } = defineMeta({
    title: 'Charts/UVIndexChart',
    component: UVIndexChart,
    tags: ['autodocs'],
    argTypes: {
      height: { control: { type: 'range', min: 150, max: 600, step: 10 } },
      date: { control: 'text' },
    },
  });
</script>

<!-- Default: sunny day peaking at UV 11 (Extreme) -->
<Story name="Extreme Day" args={{ date: 'Monday, March 30' }} />

<!-- Moderate day -->
<Story
  name="Moderate Day"
  args={{
    date: 'Tuesday, March 31',
    data: [
      { hour: 6, uv: 0 },
      { hour: 7, uv: 1 },
      { hour: 8, uv: 2 },
      { hour: 9, uv: 3 },
      { hour: 10, uv: 4 },
      { hour: 11, uv: 5 },
      { hour: 12, uv: 5 },
      { hour: 13, uv: 4 },
      { hour: 14, uv: 3 },
      { hour: 15, uv: 2 },
      { hour: 16, uv: 1 },
      { hour: 17, uv: 0 },
    ],
  }}
/>

<!-- Very High day -->
<Story
  name="Very High Day"
  args={{
    date: 'Wednesday, April 1',
    data: [
      { hour: 6, uv: 0 },
      { hour: 7, uv: 1 },
      { hour: 8, uv: 3 },
      { hour: 9, uv: 5 },
      { hour: 10, uv: 7 },
      { hour: 11, uv: 9 },
      { hour: 12, uv: 10 },
      { hour: 13, uv: 9 },
      { hour: 14, uv: 7 },
      { hour: 15, uv: 5 },
      { hour: 16, uv: 3 },
      { hour: 17, uv: 1 },
      { hour: 18, uv: 0 },
    ],
  }}
/>

<!-- Low/cloudy day -->
<Story
  name="Cloudy Day"
  args={{
    date: 'Thursday, April 2',
    data: [
      { hour: 7, uv: 0 },
      { hour: 8, uv: 1 },
      { hour: 9, uv: 1 },
      { hour: 10, uv: 2 },
      { hour: 11, uv: 2 },
      { hour: 12, uv: 2 },
      { hour: 13, uv: 1 },
      { hour: 14, uv: 1 },
      { hour: 15, uv: 0 },
    ],
  }}
/>

<!-- Tall height -->
<Story name="Tall" args={{ height: 450, date: 'Friday, April 3' }} />
