<script lang="ts">
  import { Button } from '$/components/ui/button';
  import * as Popover from '$/components/ui/popover';
  import { TID } from '$/constants';
  import { notify } from '$/util/notify';
  import { logEvent } from '$/util/stats';
  import DownloadIcon from '~icons/material-symbols/download';

  let open = $state(false);

  const clickExportButton = (testID: string): void => {
    const button = document.querySelector<HTMLButtonElement>(`[data-testid="${testID}"]`);
    if (!button) {
      notify('Export is not ready yet. Please try again.');
      return;
    }
    button.click();
    open = false;
  };

  const onExportPNG = () => {
    clickExportButton(TID.downloadPNG);
    logEvent('downloadMenu', { type: 'png' });
  };

  const onExportSVG = () => {
    clickExportButton(TID.downloadSVG);
    logEvent('downloadMenu', { type: 'svg' });
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
