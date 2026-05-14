<script lang="ts">
  import { Button } from '$/components/ui/button';
  import * as Popover from '$/components/ui/popover';
  import { notify } from '$/util/notify';
  import { urlsStore } from '$/util/state';
  import { logEvent } from '$/util/stats';
  import dayjs from 'dayjs';
  import DownloadIcon from '~icons/material-symbols/download';

  let open = $state(false);

  const getFileName = (extension: string) =>
    `mermaid-diagram-${dayjs().format('YYYY-MM-DD-HHmmss')}.${extension}`;

  const simulateDownload = (download: string, href: string): void => {
    const a = document.createElement('a');
    a.download = download;
    a.href = href;
    a.click();
    a.remove();
  };

  const onExportPNG = () => {
    if (!$urlsStore.png) {
      notify('Unable to export PNG. Please wait for the diagram to finish rendering.');
      return;
    }
    simulateDownload(getFileName('png'), $urlsStore.png);
    logEvent('download', { source: 'top_menu', type: 'png' });
    open = false;
  };

  const onExportSVG = () => {
    if (!$urlsStore.svg) {
      notify('Unable to export SVG. Please wait for the diagram to finish rendering.');
      return;
    }
    simulateDownload(getFileName('svg'), $urlsStore.svg);
    logEvent('download', { source: 'top_menu', type: 'svg' });
    open = false;
  };
</script>

<Popover.Root bind:open>
  <Popover.Trigger>
    <Button variant="outline" size="sm">
      <DownloadIcon />
      Export
    </Button>
  </Popover.Trigger>
  <Popover.Content class="w-44 p-2">
    <div class="flex flex-col gap-2">
      <Button variant="outline" onclick={onExportPNG}>PNG</Button>
      <Button variant="outline" onclick={onExportSVG}>SVG</Button>
    </div>
  </Popover.Content>
</Popover.Root>
