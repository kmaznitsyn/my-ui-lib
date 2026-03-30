<script lang="ts">
  import { Chart, Svg, Area, Spline, Axis, Highlight, LinearGradient, Tooltip } from 'layerchart';
  import { curveNatural } from 'd3';

  export interface UVDataPoint {
    /** Hour of day (0–23) */
    hour: number;
    /** UV index value */
    uv: number;
  }

  interface Props {
    /** Hourly UV index data points for the day */
    data?: UVDataPoint[];
    /** Optional date string displayed above the chart */
    date?: string;
    /** Chart height in pixels */
    height?: number;
  }

  const DEFAULT_DATA: UVDataPoint[] = [
    { hour: 5, uv: 0 },
    { hour: 6, uv: 1 },
    { hour: 7, uv: 2 },
    { hour: 8, uv: 4 },
    { hour: 9, uv: 5 },
    { hour: 10, uv: 7 },
    { hour: 11, uv: 9 },
    { hour: 12, uv: 10 },
    { hour: 13, uv: 11 },
    { hour: 14, uv: 10 },
    { hour: 15, uv: 8 },
    { hour: 16, uv: 6 },
    { hour: 17, uv: 4 },
    { hour: 18, uv: 2 },
    { hour: 19, uv: 1 },
    { hour: 20, uv: 0 },
  ];

  const UV_LEVELS = [
    { max: 2, label: 'Low', color: '#4ade80' },
    { max: 5, label: 'Moderate', color: '#facc15' },
    { max: 7, label: 'High', color: '#fb923c' },
    { max: 10, label: 'Very High', color: '#f87171' },
    { max: Infinity, label: 'Extreme', color: '#c084fc' },
  ] as const;

  function getUVLevel(uv: number) {
    return UV_LEVELS.find((l) => uv <= l.max) ?? UV_LEVELS[UV_LEVELS.length - 1];
  }

  function formatHour(hour: number): string {
    if (hour === 0) return '12am';
    if (hour === 12) return '12pm';
    return hour < 12 ? `${hour}am` : `${hour - 12}pm`;
  }

  const { data = DEFAULT_DATA, date, height = 280 }: Props = $props();

  const peakUV = $derived(Math.max(...data.map((d) => d.uv)));
  const peakLevel = $derived(getUVLevel(peakUV));
  const yMax = $derived(Math.max(12, peakUV + 1));
</script>

<div class="uv-chart-wrapper">
  {#if date}
    <p class="uv-chart-date">{date}</p>
  {/if}

  <div class="uv-chart" style="height: {height}px">
    <Chart
      {data}
      x="hour"
      y="uv"
      yDomain={[0, yMax]}
      padding={{ top: 16, right: 16, bottom: 32, left: 40 }}
      tooltip={{ mode: 'bisect-x' }}
    >
      <Svg>
        <LinearGradient id="uv-area-gradient" stops={[peakLevel.color, 'transparent']} vertical />

        <Axis
          placement="left"
          ticks={[0, 3, 6, 8, 11]}
          grid
          format={(v) => String(v)}
        />
        <Axis
          placement="bottom"
          ticks={[6, 9, 12, 15, 18]}
          format={(v) => formatHour(v)}
        />

        <Area
          fill="url(#uv-area-gradient)"
          fillOpacity={0.4}
          curve={curveNatural}
        />
        <Spline
          stroke={peakLevel.color}
          strokeWidth={2.5}
          curve={curveNatural}
        />

        <Highlight
          axis="x"
          points={{ r: 5, fill: peakLevel.color, stroke: 'white', strokeWidth: 2 }}
          lines={{ stroke: peakLevel.color, strokeWidth: 1, strokeDasharray: '4 3' }}
        />
      </Svg>

      <Tooltip.Root x="pointer" y="pointer" anchor="top-left" let:data>
        {#if data}
          {@const level = getUVLevel(data.uv)}
          <div class="uv-tooltip">
            <span class="uv-tooltip-time">{formatHour(data.hour)}</span>
            <span class="uv-tooltip-value" style="color: {level.color}">UV {data.uv}</span>
            <span class="uv-tooltip-label">{level.label}</span>
          </div>
        {/if}
      </Tooltip.Root>
    </Chart>
  </div>

  <div class="uv-legend">
    {#each UV_LEVELS as level (level)}
      <span class="uv-legend-item">
        <span class="uv-legend-dot" style="background: {level.color}"></span>
        {level.label}
      </span>
    {/each}
  </div>
</div>

<style>
  .uv-chart-wrapper {
    width: 100%;
    font-family: inherit;
  }

  .uv-chart-date {
    margin: 0 0 8px;
    font-size: 0.875rem;
    color: #6b7280;
    font-weight: 500;
  }

  .uv-chart {
    width: 100%;
    position: relative;
  }

  /* Tooltip */
  .uv-tooltip {
    background: #ffffff;
    border: 1px solid #e5e7eb;
    border-radius: 8px;
    padding: 8px 12px;
    display: flex;
    flex-direction: column;
    gap: 2px;
    box-shadow: 0 4px 12px rgb(0 0 0 / 0.1);
    pointer-events: none;
  }

  .uv-tooltip-time {
    font-size: 0.75rem;
    color: #9ca3af;
  }

  .uv-tooltip-value {
    font-size: 1.125rem;
    font-weight: 700;
    line-height: 1.2;
  }

  .uv-tooltip-label {
    font-size: 0.75rem;
    color: #6b7280;
  }

  /* Legend */
  .uv-legend {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-top: 12px;
    padding-left: 40px;
  }

  .uv-legend-item {
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 0.75rem;
    color: #6b7280;
  }

  .uv-legend-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    flex-shrink: 0;
  }
</style>
